![Common_ZK_Vulns.png](Images/Common_ZK_Vulns.png)

---

Shout out to [Kyle](https://github.com/kcharbo3) and [Gubsheep](https://twitter.com/gubsheep) for creating this resource! 

![[Kyle.png]]

![[Gubsheep.png]]

Resources - [Source here](https://github.com/0xPARC/zk-bug-tracker?utm_source=substack&utm_medium=email#bugs-in-the-wild-header)

---
### Bug 1 and 2 - [Under-constrained Circuits](https://github.com/0xPARC/zk-bug-tracker?utm_source=pocket_reader#1-under-constrained-circuits) + [Nondeterministic Circuits](https://github.com/0xPARC/zk-bug-tracker?utm_source=pocket_reader#2-nondeterministic-circuits)

This specific section of vulns is a the 80/20 rule. 20% of the vulns impact 80% of the projects - at least today, from this list. 

When programming in Circom, Ciaro and many other [ZK domain specific languages](https://zeroknowledge.fm/172-2/) (DSLs) they're [declarative](https://www.educative.io/blog/declarative-vs-imperative-programming) type languages (tell me what), not like Solidity, which is imperative (tell me how). 

![[Declaretive_Imperative.png]]

This means that we're "declaring" what we want from our program. If we miss one declaration or "constraint", then we're possibly opening ourselves up to significant consequences like allowing a user to ==repeatedly drain funds==... That's what's happening here. 

> In order to prevent these bugs, it is important to ==test== the circuit with edge cases and ==manually review== the logic in depth. 
> 
> Formal verification tools are almost production ready and will be able to catch a lot of common under-constrained bugs, such as [ECNE by 0xPARC](https://0xparc.org/blog/ecne) and [Veridise's circom-coq](https://reviewgithub.com/rep/Veridise/circom-coq).

#### Examples

##### Example 1 --> 0xPARC StealthDrop: Nondeterministic Nullifier ([link](https://github.com/0xPARC/zk-bug-tracker?utm_source=pocket_reader#6-0xparc-stealthdrop-nondeterministic-nullifier))

Detailed [Twitter thread](https://twitter.com/0xPARC/status/1493705025385906179)

> @0xB07DAD pointed out a flaw in the current StealthDrop system that allows users to ==double-claim==.

The nullifier is created through a ECDSA signature made of a private key and message. 

The ZK circuit enforces the nullifier is ==valid==, but doesn't enforce anything around the ==creation== of the nullifier. ECDSA is nondeterministic in the creation of signatures, so we're able to manipulate our raw private key and create multiple "valid" nullifiers for the same claim... Which in turn allows us to "claim" multiple drops. 

**Fix** - The ZK circuit needs to enforce proper creation of the nullifier, ensuring it's deterministic (same input = same output every time). This can be done by generating a proof on the user private key, which is commonly done through a browser based wallet ([read more here](https://twitter.com/0xPARC/status/1493705177165144069))

##### Example 2 --> [MACI](https://zeroknowledge.fm/159-2/) 1.0: Under-constrained Circuit ([link](https://github.com/0xPARC/zk-bug-tracker?utm_source=pocket_reader#7-maci-10-under-constrained-circuit))

**Background**

This is a voting dapp that allows users to vote with ZK proofs backing their validity. Each user must submit their public key to the smart contract before voting and they can't vote without the private key to that pubkey. 

Each new vote replaces the current pubkey with a new pubkey, representing that vote.

The coordinator takes the encrypted votes previously published on-chain, takes them  off-chain decrypts them, then creates a ZK proof, resubmits on-chain, this is then validated by a ZK circuit sitting on-chain. 

The coordinator is potential bad actor and the ZK circuit is missing constraints, which is where our exploit sits. 

**The Bug**

Each Pubkey should be bound to a specific "msg", which is the users vote. The circuit is not binding these two, which allows the coordinator to swap pubkeys and msgs, to censor voting (invalidate). 

**The Fix**

Ensure there's a new constraint in the ZK circuit that binds the pubkeys <--> msgs together. 

---
### Bug 3 - [Arithmetic Over/Under Flows](https://github.com/0xPARC/zk-bug-tracker?utm_source=pocket_reader#3-arithmetic-overunder-flows)

Underflow and overflow are well-known within the security community. We're either going to pop out of our designated range from the top (overflow) or the bottom (underflow)

In the case with circuits we're popping out of our range for an interesting reason, which is [modular arithmetic](https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/what-is-modular-arithmetic). <-- Check link for simple explanation. 


![[Clock_group.svg.png]]


Modular math is used heavily within ZK due to it's ability to create strong "[trap doors](https://en.wikipedia.org/wiki/Trapdoor_function)" (i.e. hard to go backwards) within the schemes we're using. 

![[trap_door.png]]

Within Circom there's a specific "prime field" that we're using for our modular math. If we go over or under, then we wrap back around, like the clock above. 

![[Circom_Prime_Field.png]]

![[Over_under_flow.png]]

##### Example 1 --> Semaphore: Missing Smart Contract Range Check ([link](https://github.com/0xPARC/zk-bug-tracker?utm_source=pocket_reader#3-semaphore-missing-smart-contract-range-check))
- Semaphore - Special club for members only

##### Example 2 --> Zk-Kit: Missing Smart Contract Range Check ([link](https://github.com/0xPARC/zk-bug-tracker?utm_source=pocket_reader#4-zk-kit-missing-smart-contract-range-check))
- ZK toolkit to assist in creating incremental merkle trees, which is a way to incrementally grow the merkle tree. This toolkit is used by Semaphore (above) to add new members to the club. 

Both examples above run into similar problems. 

**Background**
They're both overlooking a user input that has direct impact on the ZK proof (i.e. public input). This miss allows the user to maliciously or accidentally overflow the "SNARK scalar field" (i.e. our modular universe) thanks to Solidity's uint256 available integer values.  


**Fix**
The fix is simply including a require statement to ensure the user input is lower than the expected "SNARK scalar field" limit. 

![[Scalar_Field_1.png]]

![[Scalar_Field_2.png]]

---
### Bug 4 - [Mismatching Bit Lengths](https://github.com/0xPARC/zk-bug-tracker/tree/8ad138668270e0aa8dac3a3f8f5d3589d5e1281f#4-mismatching-bit-lengths)

> Many of CircomLib’s circuits take in an expected number of bits (i.e. 32 bits). In order for their constraints and output to be accurate, the input parameters need to be constrained to that maximum number of bits ==outside== of the CircomLib circuit.

##### Example --> Aztec 2.0: Missing Bit Length Check / Nondeterministic Nullifier ([link](https://github.com/0xPARC/zk-bug-tracker/tree/8ad138668270e0aa8dac3a3f8f5d3589d5e1281f#5-aztec-20-missing-bit-length-check--nondeterministic-nullifier))

**Background**
Funds are stored in "note commitments" (NC) with Aztec. Every time a NC is spent a nullifier is created alongside that NC from it's index position in the merkle tree. This index position is assumed to be a max size of 32 bits by the circuit creating the nullifier from this variable (but it's not enforced)

**Bug**
Without a bit range check on the index variable used to create the nullifier the attacker can continually update the bits outside the 32 "expected" range, spending the same NC over and over again. 

**Fix**
Enforce bit range check on the index variable before creating nullifier. 

Example
![[Maxbits.png]]



---
### Bug 5 - Unused Public Inputs Optimized out ([link](https://github.com/0xPARC/zk-bug-tracker/tree/8ad138668270e0aa8dac3a3f8f5d3589d5e1281f#5-unused-public-inputs-optimized-out))

> Many circuits will have a variable as a public input, but won’t write any constraints on that variable. These public inputs without any constraints can act as key information when verifying the proof. However, as of Circom 2.0, the default r1cs ==compilation uses an optimizer==. The optimizer will ==optimize these public inputs out of the circuit== if they are not involved in any constraints.

[Source](https://medium.com/web3studio/simple-explanations-of-arithmetic-circuits-and-zero-knowledge-proofs-806e59a79785)
![[Wires_Circuits.png]]

![[Optimizing_Out_Example.png]]

**Function of optimizer in r1cs**  
The [optimization](https://en.wikipedia.org/wiki/Optimizing_compiler) is skipping the linear "wires" (X^1), which is why the "fix" is to square all the linear "wires" (X^2) and make them non-linear, so they're not skipped. 

**More details on optimizing within r1cs** 
> [Source](https://eprint.iacr.org/2022/462.pdf): Our automated ==optimizer== allows users to focus on the circuit design and obtain efficient implementations while avoiding error-prone and complex manual optimizations. This way, the construction of circuits can be more transparent and easier to audit.

> [Linear Skip Optimization](https://eprint.iacr.org/2022/462.pdf#subsection.1.4.2): In a nutshell, we leverage the fact that the composition of linear functions is again a linear function in order to “skip” the evaluation of certain wires.

What is the difference between linear and non-linear?  [video](https://youtu.be/9To7GhdaCLY)

![[Linear_Nonlinear.png]]

**The Bug**
> Semaphore is a ZK app that allows users to prove membership of a group and send signals without revealing their identity. In this case, the signal that a user sends is hashed and included as a public input to the proof. If the Semaphore devs had not included any constraints on this variable, an ==attacker could take a valid proof of a user, modify the signal hash (public input) and replay the proof== with the modified signal hash. This is essentially ==forging any signal== they like. 

**The Fix**
> To prevent this over optimization, one can add a non-linear constraint that involves the public input (X^2). TornadoCash and Semaphore do this. TornadoCash used multiple non-linear constraints to prevent its public variables from being optimized out. Semaphore’s public “signalHash” input is intentionally added into a non-linear constraint (”signalHashSquared”) to prevent it from being optimized out.

![[Non_linear_fix_Semaphore.png]]

![[Non_linear_fix_tornado_cash.png]]


---
### Bug 6 - [Frozen Heart: Forging of Zero Knowledge Proofs](https://github.com/0xPARC/zk-bug-tracker/tree/8ad138668270e0aa8dac3a3f8f5d3589d5e1281f#6-frozen-heart-forging-of-zero-knowledge-proofs)

Head over to this video for an explanation for this Vuln. Long story short the method used to convert an interactive ZK scheme to a non-interactive scheme can lead to some serious exploits... It comes down to the implementation as always. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/ffPI0B2l2dY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

---
### Bug 7 - [Trusted Setup Leak](https://github.com/0xPARC/zk-bug-tracker/tree/8ad138668270e0aa8dac3a3f8f5d3589d5e1281f#7-trusted-setup-leak)

**Background**
> Many popular zk protocols require what is known as a ==trusted setup==. The trusted setup is used to generate the parameters ==necessary== for a prover ==to create sound zk proofs==. However, the setup also involves parameters that need to be hidden to everyone. These are known as "==toxic waste==". ==If== the toxic waste is ==revealed== to a party, then they would be ==able to forge zk proofs== for that system. The toxic waste is usually kept private in practice through the use of multi-party computation.

> Older zk protocols like Pinocchio and Groth16 require a new trusted setup for each unique circuit. This creates problems whenever a project needs to update their circuits because then they would need to redo the trusted setup. Newer protocols like ==MARLIN and PLONK== still require a trusted setup, but only once. These protocols' setup is known as "==universal==" since it can be used for multiple programs, making upgrades much easier. Other protocols such as ==Bulletproofs and STARKs== ==do not require trusted setups== at all.


##### Example --> Zcash Trusted Setup (2016 - [link](https://electriccoin.co/blog/zcash-counterfeiting-vulnerability-successfully-remediated/))

During the original setup ceremony in 2016 there were extra elements produced mistakenly, violating the protocols "soundness". See [Appendix B](https://eprint.iacr.org/2013/879.pdf)

> Some of these elements are unused by the prover and were included by mistake; but their presence ==allows a cheating== prover to circumvent a consistency check, and thereby ==transform the proof of one statement into a valid-looking proof of a different statement==. This breaks the soundness of the proving system.

![[Zcash_extra_params_edit.png]]

> The multi-party computation (MPC) protocol that produced Sprout parameters for the construction follows the paper’s setup procedure, including the computation of the extra elements. These are ==not included in the actual parameters distributed to Zcash nodes== since they are omitted from the parameter file format used by the proving routine implementation in the libsnark library (used by Sprout). ==However, these elements do appear in the MPC ceremony transcript.== Consequently, ==anyone with access to the ceremony transcript would have been able to create false proofs.== --> I.e. print free money 

[Transcript](https://github.com/zcash/mpc#transcript) - Used to verify the protocol's evaluation and construct the proving/verifying keys.

> [The point](https://forum.zcashcommunity.com/t/transcript-from-sprout-mpc-restored/31969) of the transcript is to ==prove the linkage between the six participants’s== publicly posted hashes of ==their== part in the ceremony, ==and== the resulting ==public parameters used== in the Zcash 1.0 “Sprout” between October 2016 and October 2018. If you don’t have the transcript, you can’t verify that those six public hashes match the original parameters, which means someone (for example, someone who had hacked into one of the Zcash Company’s laptops) could have substituted their own parameters (for which they could have the toxic waste) in place of the parameters that the six participants collectively generated.
