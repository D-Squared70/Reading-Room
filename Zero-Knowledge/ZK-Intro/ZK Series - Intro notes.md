### Outline (started from [here](https://en.wikipedia.org/wiki/Zero-knowledge_proof "https://en.wikipedia.org/wiki/Zero-knowledge_proof"))

---
Intro Series - Purpose, public learning expectation setting, resource sharing (highlight existing GitHub pages), and mention weekly email newsletter (ZK, crypto, and random)
- _**Important preface!** We're taking small bites from this monster of a topic. Going forward the topics themselves should be semi-tight, so we're not getting overwhelmed or stretching too thin. The goal is to make this process sustainable and consistent, which can be easily overcome by ambition._ 

Intro ZK
- One-sentence definition
- Why is it useful?  
	- [1](https://arxiv.org/pdf/1906.07221.pdf?utm_source=pocket_saves) > abstract applications
	- [2](https://youtu.be/-2qHqfqPeR8) > distributed compute (outsource to AWS, but verify they computed what's expected)
	- 3 ==< Need more== 
- List of silly examples, ranking them on spectrum of intuitive to accurate

Elements - Walk through the main elements of, but keep the scope down to 6 elements max **_(largest amount of time here)_**
-   Security Lens - Make sure to bring in the security lens sharing the [Trail of Bits intro blog](https://blog.trailofbits.com/2021/02/19/serving-up-zero-knowledge-proofs/?utm_source=pocket_reader) post and weaknesses in specific elements of ZK 
-   [Vuln reading](https://kb.delendum.xyz/zk-knowledge#vulnerabilities)

Moon Math - Share the main types of moon math needed to truly understand the inner workings, but preface that we're only going deep enough to intuit whats happening
- Outro - Share resources once more, mention GitHub link in description, and mention weekly email newsletter again

---

### Notes

What is ZK?
-   Intro the main concept through silly example
	- Examples ([wiki](https://en.wikipedia.org/wiki/Zero-knowledge_proof#Abstract_examples "https://en.wikipedia.org/wiki/Zero-knowledge_proof#Abstract_examples")): Two different colored balls, Ring cave, [Where’s Waldo](https://blog.goodaudience.com/understanding-zero-knowledge-proofs-through-simple-examples-df673f796d99 "https://blog.goodaudience.com/understanding-zero-knowledge-proofs-through-simple-examples-df673f796d99"), [Hats example](https://blog.cryptographyengineering.com/2014/11/27/zero-knowledge-proofs-illustrated-primer/?utm_source=pocket_reader "https://blog.cryptographyengineering.com/2014/11/27/zero-knowledge-proofs-illustrated-primer/?utm_source=pocket_reader"), colored map (no same colored countries boarding), ==Not Boring== [Post](https://www.notboring.co/p/zero-knowledge) - examples near the ending
		- Shout out to "Not Boring", very well crafted blog/newsletter series 
	- Briefly touch on the history of ZK

What are the main elements of ZK?
-   [Interactive](https://en.m.wikipedia.org/wiki/Interactive_proof_system "https://en.m.wikipedia.org/wiki/Interactive_proof_system") vs. [non-interactive](https://en.m.wikipedia.org/wiki/Non-interactive_zero-knowledge_proof "https://en.m.wikipedia.org/wiki/Non-interactive_zero-knowledge_proof") proofs (mainly non-interactive for crypto - Why? Efficiency and ultimately scale, I think)
-   ZK must satisfy three properties: Soundness (iterations), Completeness (convince me), and Zero-knowledge (show nothing)
-   Provers can be [ranked](https://youtu.be/BT88s7_VtC8?t=357) (see image below) - Prover time, Proof size, Verification time
-   Different protocols - Highlight which are being used by the main players, preferably through a graph showing the different systems (Monero, Zcash, ZKSyncn, Starkwar, etc. - [Good preso](https://youtu.be/BT88s7_VtC8 "https://youtu.be/BT88s7_VtC8") on history of protocols and which are being used by each system) ==< BETTER GRAPH/EXPLANATION==
	- ZK-SNARK - Polynomials sit at the center of this protocol which makes it efficient and non-interactive, both traits needed for scale within crypto use cases - See "[The Medium of proof](https://arxiv.org/pdf/1906.07221.pdf?utm_source=pocket_saves)" section
	- ZK-STARK - No secret ceremony needed, which is "T" for transparent. "S" stands for scale due to the proof size, time, and verifier time all being much faster than SNARKs.  
	- Proper [breakdown of differences here](https://medium.com/@krzhang/privacy-in-cryptocurrencies-zero-knowledge-and-zk-snarks-part-2-f6a3ee167b16) near the beginning of the part 2 blog post
	- SNARK vs. STARK (2 images below pulled [from here](https://medium.com/amber-group/navigating-zero-knowledge-e944b21af71c))
![[Pasted image 20221220120557.png]]
![[Pasted image 20221220120645.png]]

- Applications of ZK within crypto ([from here](https://www.notboring.co/p/aleo-can-you-keep-a-secret))
	![[Pasted image 20221220095253.png]]
-   ZK Systems vs. Protocol - In general, a zero-knowledge protocol is the broader concept, while a zero-knowledge system is a specific way of implementing a zero-knowledge protocol. [See table here](https://en.wikipedia.org/wiki/Zero-knowledge_proof#Zero-Knowledge_Proof_protocols "https://en.wikipedia.org/wiki/Zero-knowledge_proof#Zero-Knowledge_Proof_protocols").
	- Walk through the main protocols and common implementations used in crypto today

What type of “moon math” does someone need to grok to intuit the engine beneath ZK?
-   Joke about the “moon math” (silly photo or graphic) and how we’re going to grok our way through just enough to gain intuition as to what’s happening under the hood
	-  Polynomials ([intro](https://youtu.be/Vm7H0VTlIco "https://youtu.be/Vm7H0VTlIco") Khan Academy
		- Porters [explanation](https://youtu.be/-2qHqfqPeR8?t=874) benefits/application
		- Section "Proving Knowledge of a Polynomial" [understandable write-up](https://arxiv.org/pdf/1906.07221.pdf?utm_source=pocket_saves)
	-  Modulo arithmetic (section - [3.3.2 Modular Arithmetic](https://arxiv.org/pdf/1906.07221.pdf?utm_source=pocket_saves)) < rope example  
	-  What else??? 

Prover quality 



![Screenshot from 2022-12-19 09-10-35.png](file:///home/dd/.config/joplin-desktop/resources/aee9f2914f2a44ec892b4143c5b6dcbf.png)

- Is trusted setup needed? Older ZK systems had one major downside, which is “trusted setups” (NOTE: Dedicate a video to this topic alone). Research has boomed recently to create systems without this need.  - _Note: Highlighted are the ones that have the most attention_

![7d9ae261bbed76ddc3b86a951767e9e6.png](file:///home/dd/.config/joplin-desktop/resources/f488042f493e47f6983561b166e700f3.png)

---

### Useful Resources

Create a running list of resources I found useful or inspirational throughout this journey - Keep in a seperate Github folder

Inspirational
- VK predicting the importance of ZK in the future. Just as important as Blockchain tech - [Video clip](https://youtu.be/hBupNf1igbY?t=2075 "https://youtu.be/hBupNf1igbY?t=2075")

Useful
-   [Wiki article](https://en.wikipedia.org/wiki/Zero-knowledge_proof "https://en.wikipedia.org/wiki/Zero-knowledge_proof")
-   [ZK at 5 levels](https://www.youtube.com/watch?v=fOGdb1CTu5c&list=WL&index=1 "https://www.youtube.com/watch?v=fOGdb1CTu5c&list=WL&index=1")
-   Privacy in Cryptocurrencies: An Overview ([P1](https://medium.com/@yi.sun/privacy-in-cryptocurrencies-d4b268157f6c), [P2](https://medium.com/@yi.sun/privacy-in-cryptocurrencies-mixing-based-approaches-ce08d0040c88), [P3](https://medium.com/@krzhang/privacy-in-cryptocurrencies-zero-knowledge-and-zk-snarks-1-2-68ce1838fd9c), [P4](https://medium.com/@krzhang/privacy-in-cryptocurrencies-zero-knowledge-and-zk-snarks-part-2-f6a3ee167b16))
	-   Practical perspective on how ZK is/can be leveraged for crypto
- [ZK Rollup](https://medium.com/ppio/zk-rollup-making-scalable-blockchains-possible-7308b695d929): Making Scalable Blockchains Possible 
> 	 - Strong explanation covering the basics of ZK roll-ups and how they function
- [Navigating](https://medium.com/amber-group/navigating-zero-knowledge-e944b21af71c) ZK Roll-ups: Strong piece breaking down the ZK roll-up ecosystem

Ultimate List of Lists(request PRs)
-   [ZK Starter Pack List](https://ethresear.ch/t/zero-knowledge-proofs-starter-pack/4519)
-   [ZK Awesome List](https://github.com/matter-labs/awesome-zero-knowledge-proofs) (ZKsync)
-   [ZK Awesome List](https://github.com/ventali/awesome-zk) (Ventali)
-   [ZK Course](https://learn.0xparc.org/circom/) from 0xparc
	-   YouTube lecture series available 
- Porter [ZK Course](https://youtu.be/-2qHqfqPeR8)
-   [Awesome list](https://github.com/0xJuancito/awesome-zksync) about ZKsync
-   [Learning ZKPs: Beginner Resources](https://community.zeroknowledge.fm/t/learning-zkps-beginner-resources/302/1) (ZK podcast community thread)
	- [Top Theoretical](https://www.youtube.com/playlist?list=PLGkwtcB-DfpzST-medFVvrKhinZisfluC) - Foundations of Probabilistic Proofs (Fall 2020) - Alessandro Chiesa
	- [Top Practical](https://www.youtube.com/watch?v=8WVW5DCVQe0&list=PLgKuh-lKre10OEVNLH3t0QX0rIK8kK3tu) - Efficient Zero-Knowledge Proofs: A Modular Approach  
                 

Moon Math
-   How and why ZK-SNARKS work [blog series](https://medium.com/@imolfar) and [PDF version](https://arxiv.org/pdf/1906.07221.pdf)
-   3Blue1Brown [video](https://youtu.be/bOXCLR3Wric) - Olympiad level counting: How many subsets of {1,…,2000} have a sum divisible by 5?

---

Name ideas... 

-   Bite size ZK
-   Snack size ZK
-   ZK snacks
-   Learning in public
-   Bite Size Brain Pickings
-   Micro ZK Lessons
