# Property-Based Testing Bibliography

This bibliography aims to cover the literature on _property-based testing_ (PBT) à la [Haskell QuickCheck](https://doi.org/10.1145/351240.351266)       
(inspired by Jeremy Yallop's [algebraic effects bibliography](https://github.com/yallop/effects-bibliography) & Sam Hobin-Hochstadt's [gradual typing bibliography](https://github.com/samth/gradual-typing-bib)).       

Pull requests welcome! Note: end a line with two spaces to force a line break.         

[![Build Status](https://github.com/ngernest/pbt-bibliography/actions/workflows/links.yml/badge.svg)](https://github.com/ngernest/pbt-bibliography/actions/workflows/links.yml)

**Contents**:
- [Papers](#papers)
- [Dissertations](#dissertations)

***

## Papers
### 2025 
* **Fulminate: Testing CN Separation-Logic Specifications in C** (POPL 2025)             
  Rini Banerjee, Kayvan Memarian, Dhruv Makwana, Christopher Pulte, Neel Krishnaswami, Peter Sewell                       
  ([pdf](https://www.cl.cam.ac.uk/~cp526/2024-cn-testing-paper.pdf))   

* **Teaching Software Specification (Experience Report)** (ICFP 2025)                  
  Cameron Moy, Daniel Patterson           
  ([pdf](https://dbp.io/pubs/2025/lsl.pdf))           

* **Tuning Random Generators: Property-Based Testing as Probabilistic Programming** (OOPSLA 2025)           
  Ryan Tjoa, Poorva Garg, Harrison Goldstein, Todd Millstein, Benjamin C. Pierce, Guy Van den Broeck             
  ([pdf](https://arxiv.org/pdf/2508.14394))         

* **We’ve Got You Covered: Type-Guided Repair of Incomplete Input Generators** (OOPSLA 2025)         
  Patrick LaFontaine, Zhe Zhou, Ashish Misra, Suresh Jagannathan, Benjamin Delaware                          
  ([pdf](https://arxiv.org/pdf/2504.06421))

* **Bennet: Randomized Specification Testing for Heap-Manipulating Programs** (OOPSLA 2025)           
  Zain K. Aamer, Benjamin C. Pierce       

* **An Empirical Evaluation of Property-Based Testing in Python** (OOPSLA 2025)         
  Savitha Ravi, Michael Coblenz

* **Lightweight Testing of Persistent Amortized Time Complexity in the Credit Monad** (Haskell 2025)             
  Anton Lorenzon          
  ([pdf](https://antonlorenzen.de/papers/creditmonad.pdf))           
  
* **Ratte: Fuzzing for Miscompilations in Multi-Level Compilers Using Composable Semantics** (ASPLOS 2025)         
  Pingshi Yu, Nicholas Wu, Alastair F. Donaldson        
  ([doi](https://doi.org/10.1145/3676641.3716270))

* **Ghost in the Android Shell: Pragmatic Test-oracle Specification of a Production Hypervisor** (SOSP 2025)           
  Kayvan Memarian, Ben Simner, David Kaloper-Meršinjak, Thibaut Pérami, Peter Sewell        
  ([pdf](https://2plus2a.com/files/publications/2025-SOSP-execspec.pdf))      

* **The Havoc Paradox in Generator-Based Fuzzing** (ACM TOSEM 2025)                
  Ao Li, Madonna Huang, Vasudev Vikram, Caroline Lemieux, Rohan Padhye             
  ([doi](https://doi.org/10.1145/3742894))

* **Property-based Testing of Attribute Grammars** (SLE 2025)           
  José Nuno Macedo, Marcos Viera, João Saraiva          
  ([doi](https://doi.org/10.1145/3732771.3742710))

* **TerzoN: Human-in-the-Loop Software Testing with a Composite Oracle** (FSE 2025)            
  Matthew C. Davis, Amy Wei, Brad A. Myers, Joshua Sunshine      
  ([doi](https://doi.org/10.1145/3729359))

* **Fail Faster: Staging and Fast Randomness for High-Performance PBT** (under submission)                                   
  Cynthia Richey*, Joseph W. Cutler*, Harrison Goldstein, Benjamin C. Pierce           
  ([pdf](https://www.cis.upenn.edu/~jwc/assets/fail-faster.pdf))    

### 2024 
* **Generating Well-Typed Terms that are not "Useless"** (POPL 2024)     
  Justine Frank, Benjamin Quiring, Leonidas Lampropoulos      
  ([doi](https://dl.acm.org/doi/10.1145/3632919))    

* **Property-Based Testing in Practice** (ICSE 2024)    
  Harrison Goldstein, Joseph W. Cutler, Daniel Dickstein, Benjamin C. Pierce, Andrew Head    
  ([doi](https://dl.acm.org/doi/10.1145/3597503.3639581))           

* **How We Built Cedar: A Verification-Guided Approach** (FSE 2024)                     
  Craig Disselkoen, Aaron Eline, Shaobo He, Kyle Headley, Mike Hicks, Kesha Hietala, John Kastner, Anwar Mamat, Matt McCutchen, Neha Rungta, Bhakti Shah, Emina Torlak, Andrew Wells                  
  ([doi](https://doi.org/10.1145/3663529.3663854))

* **Practical Verification of System-Software Components Written in Standard C** (SOSP 2024)        
  Can Cebeci, Yonghao Zou, Diyu Zhou, George Candea, Clément Pit-Claudel        
  ([doi](https://doi.org/10.1145/3694715.3695980))   

* **Property-Based Testing by Elaborating Proof Outlines** (TPLP 2024)         
  Dale Miller, Alberto Momigliano               
  ([doi](https://doi.org/10.1017/S1471068424000176))       

* **Type-Level Property Based Testing** (TyDe 2024)   
  Thomas Ekström Hansen, Edwin Brady    
  ([doi](https://dl.acm.org/doi/10.1145/3678000.3678206))      

* **Dependent Types to Push Corners of the Property-based Testing** (TyDe 2024)      
  Denis Buzdalov    
  ([link](https://icfp24.sigplan.org/details/tyde-2024-papers/6/Dependent-Types-to-Push-Corners-of-the-Property-based-Testing-Extended-Abstract-))   

* **Mica: Automated Differential Testing for OCaml Modules** (OCaml 2024)    
  Ernest Ng, Harrison Goldstein, Benjamin C. Pierce    
  ([arXiv](https://www.arxiv.org/abs/2408.14561))   

* **Tyche: Making Sense of PBT Effectiveness** (UIST 2024)          
  Harrison Goldstein, Jeffrey Tao, Zac Hatfield-Dodds, Benjamin C. Pierce, Andrew Head     
  ([doi](https://dl.acm.org/doi/10.1145/3654777.3676407))

* **Rustlantis: Randomized Differential Testing of the Rust Compiler** (OOPSLA 2024)          
  Qian Wang, Ralf Jung          
  ([doi](https://doi.org/10.1145/3689780))

* **General and Practical Property-based Testing for Android Apps** (ASE 2024)     
  Yiheng Xiong, Ting Su, Jue Wang, Jingling Sun, Geguang Pu, Zhendong Su      
  ([doi](https://doi.org/10.1145/3691620.3694986))        

* **Evaluating PBT Frameworks in OCaml** (PLDI SRC 2024)       
  Nikhil Kamath     
  ([pdf](https://drive.google.com/file/d/1GeH4SOIejfeKQfLbvpaN-nKi6inxhs_g/view))

* **Constrained generation of well-typed program**     
  Gabriel Scherer       
  ([doi](https://inria.hal.science/hal-04607309v1))

* **Can Large Language Models Write Good Property-Based Tests?**     
  Vasudev Vikram, Caroline Lemieux, Joshua Sunshine, Rohan Padhye      
  ([arXiv](https://arxiv.org/abs/2307.04346))
  

### 2023 
* **Covering All the Bases: Type-Based Verification of Test Input Generators** (PLDI 2023)     
  Zhe Zhou, Ashish Mishra, Benjamin Delaware, Suresh Jagannathan        
  ([doi](https://doi.org/10.1145/3591271))    

* **Merging Inductive Relations** (PLDI 2023)        
  Jacob Prinz, Leonidas Lampropoulos        
  ([doi](https://dl.acm.org/doi/10.1145/3591292))

* **Mostly Automated Proof Repair for Verified Libraries** (PLDI 2023)                 
  Kiran Gopinathan, Mayank Keoliya, Ilya Sergey                      
  ([doi](https://doi.org/10.1145/3591221))              

* **Reflecting on Random Generation** (ICFP 2023)    
  Harrison Goldstein, Samantha Frohlich, Meng Wang, Benjamin C. Pierce        
  ([doi](https://doi.org/10.1145/3607842))         

* **Etna: An Evaluation Platform for Property-Based Testing** (ICFP 2023)     
  Jessica Shi, Alperen Keles, Harrison Goldstein, Benjamin C. Pierce, Leonidas Lampropoulos       
  ([doi](https://doi.org/10.1145/3607860))           

* **Formal Specification and Testing for Reinforcement Learning** (ICFP 2023)          
  Mahsa Varshosaz, Mohsen Ghaffari, Einar Broch Johnsen, Andrzej Wąsowski         
  ([doi](https://doi.org/10.1145/3607835))

* **Formalizing Stack Safety as a Security Property** (CSF 2023)              
  Sean Noble Anderson, Roberto Blanco, Leonidas Lampropoulos, Benjamin C. Pierce, Andrew Tolmach                
  ([doi](https://doi.org/10.48550/arXiv.2105.00417))               

* **QuickerCheck: Implementing and Evaluating a Parallel Run-Time for QuickCheck** (IFL 2023)     
  Robert Krook, Nicholas Smallbone, Bo Joel Svensson, Koen Claessen    
  ([doi](https://dl.acm.org/doi/10.1145/3652561.3652570))      

* **Don’t Go Down the Rabbit Hole: Reprioritizing Enumeration for Property-Based Testing** (Haskell 2023)     
  Segev Elazar Mittelman, Alvin Resnick, Ivan Perez, Alwyn Goodloe, Leonidas Lampropoulos        
  ([doi](https://doi.org/10.1145/3609026.3609730))      

* **falsify: Internal Shrinking Reimagined for Haskell** (Haskell 2023)     
  Edsko de Vries        
  ([doi](https://doi.org/10.1145/3609026.3609733))          

* **How Developers Implement Property-Based Tests** (ICSME 2023)      
  Arthur Lisboa Corgozinho,Marco Tulio Valente, Henrique Rocha          
  ([doi](https://ieeexplore.ieee.org/document/10336336))     

* **Evaluating Soundness of a Gradual Verifier with Property Based Testing** (POPL 2023 SRC)        
  Jan-Paul Ramos-Dávila          
  ([pdf](https://janpaul.pl/assets/pdfs/popl_src23.pdf))        

### 2022
* **Logarithm and Program Testing** (POPL 2022)    
  Kuen-Bang Hou (Favonia), Zhuyang Wang       
  ([doi](https://dl.acm.org/doi/10.1145/3498726))

* **Quickstrom: property-based acceptance testing with LTL specifications** (PLDI 2022)        
  Liam O'Connor, Oskar Wickström      
  ([doi](https://doi.org/10.1145/3519939.3523728))

* **Computing correctly with inductive relations** (PLDI 2022)              
  Zoe Paraskevopoulou, Aaron Eline, Leonidas Lampropoulos             
  ([doi](https://doi.org/10.1145/3519939.3523707))                     

* **A Completely Unique Account of Enumeration** (ICFP 2022)   
  Cas van der Rest, Wouter Swierstra    
  ([doi](https://dl.acm.org/doi/pdf/10.1145/3547636))    

* **Random Testing of a Higher-Order Blockchain Language** (ICFP 2022)     
  Tram Hoang, Anton Trunov, Leonidas Lampropoulos, Ilya Sergey        
  ([doi](https://dl.acm.org/doi/10.1145/3547653))       

* **Parsing Randomness** (OOPSLA 2022)      
  Harrison Goldstein, Benjamin C. Pierce       
  ([doi](https://dl.acm.org/doi/abs/10.1145/3563291))

* **Some Problems with Properties: A Study on Property-Based Testing in Industry** (HATRA 2022)    
  Harrison Goldstein, Joseph W. Cutler, Adam Stein, Andrew Head, Benjamin C. Pierce      
  ([pdf](https://harrisongoldste.in/papers/hatra2022.pdf))          

* **Automated, Targeted Testing of Property-Based Testing Predicates** 
  (The Art, Science, and Engineering of Programming, 2022)          
  Tim Nelson, Elijah Rivera, Sam Soucie, Thomas Del Vecchio, Jack Wrenn, Shriram Krishnamurthi          
  ([pdf](https://arxiv.org/pdf/2111.10414))                    

* **Functional Pearl: Holey Generators!**    
  Joseph W. Cutler, Harrison Goldstein, Koen Claessen, John Hughes, Benjamin C. Pierce    
  ([pdf](https://www.cis.upenn.edu/~jwc/assets/holey.pdf))

* **Learning Good Generators for Property-Based Testing** (PLDI 2022 SRC)        
  Joseph W. Cutler         
  ([pdf](https://www.cis.upenn.edu/~jwc/assets/bandits.pdf))  

### 2021
* **Do Judge a Test by its Cover: Combining Combinatorial and Property-Based Testing** (ESOP 2021)      
  Harrison Goldstein, John Hughes, Leonidas Lampropoulos, Benjamin C. Pierce      
  ([doi](https://dl.acm.org/doi/10.1007/978-3-030-72019-3_10))

* **Data-Driven Abductive Inference of Library Specifications** (OOPSLA 2021)      
  Zhe Zhou, Robert Dickerson, Benjamin Delaware, Suresh Jagannathan      
  ([doi](https://dl.acm.org/doi/10.1145/3485493))      

* **Using Lightweight Formal Methods to Validate a Key-Value Storage Node in Amazon S3** (SOSP 2021)     
  James Bornholt, Rajeev Joshi, Vytautas Astrauskas, Brendan Cully, Bernhard Kragl, Seth Markle, Kyle Sauri, Drew Schleit, Grant Slatton, Serdar Tasiran, Jacob Van Geffen, Andrew Warfield         
  ([doi](https://dl.acm.org/doi/10.1145/3477132.3483540))

* **Resolvable Ambiguity: Principled Resolution of Syntactically Ambiguous Programs** (CC 2021)               
  Viktor Palmkvist, Elias Castegren, Philipp Haller, David Broman           
  ([doi](https://doi.org/10.1145/3446804.3446846))           

* **Strong automated testing of OCaml libraries** (JFLA 2021)    
  François Pottier         
  ([pdf](https://inria.hal.science/hal-03049511v1/file/pottier-monolith-2021.pdf))          

* **Parafuzz: Coverage-guided Property Fuzzing for Multicore OCaml programs** (OCaml 2021)      
  Sumit Padhiyar, Adharsh Kamath, KC Sivaramakrishnan         
  ([url](https://icfp21.sigplan.org/details/ocaml-2021-papers/9/Parafuzz-Coverage-guided-Property-Fuzzing-for-Multicore-OCaml-programs))

* **Property-Based Testing for OCaml through Coq** (OCaml 2021)              
  Paaras Bhandari, Leonidas Lampropoulos          
  ([pdf](https://lemonidas.github.io/pdf/TestingOcamlThroughCoq.pdf))        

* **Using Relational Problems to Teach Property-Based Testing**         
  (The Art, Science, and Engineering of Programming, 2021)    
  John Wrenna, Tim Nelsona, Shriram Krishnamurthi       
  ([doi](https://programming-journal.org/2021/5/9/))

* **PyH2: Using PyMTL3 to Create Productive and Open-Source Hardware Testing Methodologies** (IEEE Design & Test 2021)            
  Shunning Jiang, Yanghui Ou, Peitian Pan, Kaishuo Cheng, Yixiao Zhang, Christopher Batten              
  [pdf](https://www.csl.cornell.edu/~cbatten/pdfs/jiang-pyh2-dt2021.pdf)     
  
### 2020 
- **Test-Case Reduction via Test-Case Generation: Insights from the Hypothesis Reducer** (ECOOP 2020)          
  David R. MacIver, Alastair F. Donaldson               
  ([doi](https://doi.org/10.4230/LIPIcs.ECOOP.2020.13))                       

* **Quickly Generating Diverse Valid Test Inputs with Reinforcement Learning** (ICSE 2020)      
  Sameer Reddy, Caroline Lemieux, Rohan Padhye, Koushik Sen         
  ([doi](https://doi.org/10.1145/3377811.3380399))         

* **A Simple State-Machine Framework for Property-Based Testing in OCaml** (OCaml 2020)     
  Jan Midtgaard    
  ([pdf](https://janmidtgaard.dk/papers/Midtgaard%3aOCaml20.pdf))

* **Finding and understanding bugs in FPGA synthesis tools** (FPGA 2020)         
  Yann Herklotz, John Wickerson          
  ([doi](https://dl.acm.org/doi/10.1145/3373087.3375310))

### 2019 
* **Coverage guided, property based testing** (OOPSLA 2019)       
  Leonidas Lampropoulos, Michael Hicks, Benjamin C. Pierce       
  ([doi](https://dl.acm.org/doi/10.1145/3360607))

* **Deriving compositional random generators** (IFL 2019)            
  Agustín Mista, Alejandro Russo        
  ([doi](https://doi.org/10.1145/3412932.3412943))

* **Semantic Fuzzing with Zest** (ISSTA 2019)               
  Rohan Padhye, Caroline Lemieux, Koushik Sen, Mike Papadakis, Yves Le Traon               
  ([doi](https://doi.org/10.1145/3339069))               

### 2018 
* **Generating Good Generators for Inductive Relations** (POPL 2018)     
  Leonidas Lampropoulos, Zoe Paraskevopoulou, Benjamin C. Pierce        
  ([doi](https://dl.acm.org/doi/10.1145/3158133))      
    
* **Keep your laziness in check** (ICFP 2018)     
  Kenny Foner, Hengchu Zhang, Leonidas Lampropulos      
  ([doi](https://dl.acm.org/doi/10.1145/3236797))

* **Branching processes for QuickCheck generators** (Haskell 2018)              
  Agustín Mista, Alejandro Russo, John Hughes             
  ([doi](https://doi.org/10.1145/3242744.3242747))           

* **AutoBench: Comparing the Time Performance of Haskell Programs** (Haskell 2018)         
  Martin A. T. Handley, Graham Hutton          
  ([doi](https://doi.org/10.1145/3299711.3242749))

* **Cheap Remarks about Concurrent Programs** (FLOPS 2018)          
  Michael Walker, Colin Runciman              
  ([doi](https://doi.org/10.1007/978-3-319-90686-7_17))            

* **Property Based Testing of C Code from Haskell** (Bachelor's Thesis, University of New South Wales)            
  Alexander Hodges                
  ([pdf](https://people.eng.unimelb.edu.au/rizkallahc/theses/alex-hodges-honours-thesis.pdf))           

### 2017
* **Beginner's luck: a language for property-based generators** (POPL 2017)    
  Leonidas Lampropoulos, Diane Gallois-Wong, Cătălin Hriţcu, John Hughes, Benjamin C. Pierce, Li-yao Xia     
  ([pdf](https://lemonidas.github.io/pdf/Luck.pdf))                       

* **Effect-driven QuickChecking of compilers** (ICFP 2017)       
  Jan Midtgaard, Mathias Nygaard Justesen, Patrick Kasting, Flemming Nielson, Hanne Riis Nielson       
  ([doi](https://doi.org/10.1145/3110259))

* **Ode on a Random Urn (Functional Pearl)** (Haskell 2017)       
  Leonidas Lampropoulos, Antal Spector-Zabusky, Kenny Foner    
  ([doi](https://doi.org/10.1145/3122955.3122959))      

* **Speculate: discovering conditional equations and inequalities about black-box functions by reasoning from test results** (Haskell 2017)          
  Rudy Braquehais, Colin Runciman          
  ([doi](https://doi.org/10.1145/3122955.3122961))        

* **Extrapolate: generalizing counterexamples of functional test properties** (IFL 2017)            
  Rudy Braquehais, Colin Runciman                  
  ([doi](https://doi.org/10.1145/3205368.3205371))             

* **QuickChecking Patricia Trees** (TFP 2017)    
  Jan Midtgaard    
  ([pdf](https://janmidtgaard.dk/papers/Midtgaard%3aTFP17.pdf))            

* **Quick Specifications for the Busy Programmer** (JFP 2017)      
  Nicholas Smallbone, Moa Johansson, Koen Claessen, Maximilian Algehed         
  ([doi](https://www.cambridge.org/core/journals/journal-of-functional-programming/article/quick-specifications-for-the-busy-programmer/1E1E8A11F91A927DD56AF08280A9F58D))

* **Fair Enumeration Combinators** (JFP 2017)                
  Max S. New, Burke Fetscher, Robert Bruce Findler, Jay McCarthy                
  ([doi](https://doi.org/10.1017/S0956796817000107)) 

* **Failing faster: overlapping patterns for property-based testing** (PADL 2017)      
  Jonathan Fowler, Graham Hutton             
  ([doi](https://doi.org/10.1007/978-3-319-51676-9_7))

* **ChimpCheck: Property-Based Randomized Test Generation for Interactive Apps** (Onward! 2017)
  Edmund S. L. Lam, Peilun Zhang, Bor-Yuh Evan Chang       
  ([doi](https://doi.org/10.1145/3133850.3133853))           

* **Testing with Crowbar** (OCaml 2017)     
  Stephen Dolan, Mindy Preston      
  ([url](https://icfp17.sigplan.org/details/ocaml-2017-talks/12/Testing-with-Crowbar))     

* **A simple incremental development of a property-based testing tool (Functional Pearl)**      
  Rudy Braquehais, Michael Walker, José Manuel Calderón Trilla, Colin Runciman    
  ([pdf](http://jmct.cc/pearlcheck.pdf))            

### 2016 
* **FitSpec: refining property sets for functional testing** (Haskell 2016)     
  Rudy Braquehais, Colin Runciman      
  ([doi](https://doi.org/10.1145/2976002.2976003))

* **QuickFuzz: an automatic random fuzzer for common file formats** (Haskell 2016)           
  Gustavo Grieco, Martín Ceresa, Pablo Buiras           
  ([doi](https://doi.org/10.1145/2976002.2976017))             

* **Mysteries of DropBox: Property-Based Testing of a Distributed Synchronization Service** (ICST 2016)    
  John Hughes, Benjamin C. Pierce, Thomas Arts, Ulf Norell       
  ([doi](https://ieeexplore.ieee.org/document/7515466))       

* **Testing Noninterference, Quickly** (JFP 2016, appeared initially at ICFP 2013)           
  Cătălin Hriţcu, Leonidas Lampropoulos, Antal Spector-Zabusky, Arthur Azevedo Amorim, Maxime Denes, John Hughes, Benjamin C. Pierce, Dimitrios Vytiniotis     
  ([doi](https://doi.org/10.1017/S0956796816000058))           

* **Experiences with QuickCheck: Testing the Hard Stuff and Staying Sane**                  
  (*A List of Successes That Can Change the World*, Essays Dedicated to Philip Wadler on the Occasion of His 60th Birthday)      
  John Hughes       
  ([doi](https://link.springer.com/chapter/10.1007/978-3-319-30936-1_9))        

* **CurryCheck: Checking Properties of Curry Programs** (LOPSTR 2016)             
  Michael Hanus         
  ([doi](https://doi.org/10.1007/978-3-319-63139-4_13))             

* **Formalising Luck: Improved Probabilistic Semantics for Property-Based Generators** (INRIA Internship Report)          
  Diane Gallois-Wong, Cătălin Hriţcu           
  ([pdf](https://catalin-hritcu.github.io/students/diane/report.pdf))                 

### 2015 
* **Making Random Judgments: Automatically Generating Well-Typed Terms from the Definition of a Type-System** (ESOP 2015)      
  Burke Fetscher, Koen Claessen, Michał Pałka, John Hughes, Robert Bruce Findler        
  ([doi](https://doi.org/10.1007/978-3-662-46669-8_16))

* **Type Targeted Testing** (ESOP 2015)             
  Eric L. Seidel, Niki Vazou, Ranjit Jhala                
  ([doi](https://doi.org/10.1007/978-3-662-46669-8_33))          

* **Generating constrained random data with uniform distribution** (JFP 2015)        
  Koen Claessen, Jonas Duregård, Michał Pałka                   
  ([doi](https://doi.org/10.1017/S0956796815000143)        

* **Foundational Property-Based Testing** (ITP 2015)      
  Zoe Paraskevopoulou, Cătălin Hriţcu, Maxime Dénès, Leonidas Lampropoulos, Benjamin C. Pierce     
  ([doi](https://link.springer.com/chapter/10.1007/978-3-319-22102-1_22))

* **Déjà Fu: a concurrency testing library for Haskell** (Haskell 2015)            
  Michael Walker, Colin Runciman           
  ([doi](https://doi.org/10.1145/2887747.2804306))           

* **Quickchecking Static Analysis Properties** (ICST 2015)        
  Jan Midtgaard, Anders Møller     
  ([pdf](https://janmidtgaard.dk/papers/Midtgaard-Moeller%3aSTVR17.pdf))             

* **Towards a Theory of Reach** (TFP 2015)      
  Jonathan Fowler, Graham Hutton      
  ([doi](https://doi.org/10.1007/978-3-319-39110-6_2))     

* **Testing AUTOSAR software with QuickCheck** (ICSTW 2015)            
  Thomas Arts, John Hughes, Ulf Norell, Hans Svensson              
  ([doi](https://ieeexplore.ieee.org/document/7107466))

* **A Generic Synthesisable Test Bench** (MEMOCODE 2015)         
  Matthew Naylor, Simon Moore         
  ([pdf](https://www.cl.cam.ac.uk/~swm11/research/papers/memocode2015.pdf)) 

* **Integrating Functional Logic Programming with Constraint Solving for Random Generation of Structured Data** (INRIA Internship Report)     
  Li-yao Xia, Cătălin Hriţcu              

### 2014 
- **SmartCheck: automatic and efficient counterexample reduction and generalization** (Haskell 2014)           
  Lee Pike        
  ([doi](https://doi.org/10.1145/2775050.2633365))          

- **A Coq Framework For Verified Property-Based Testing** (INRIA Internship Report 2014)          
  Zoe Paraskevopoulou, Cătălin Hriţcu       
  ([pdf](https://catalin-hritcu.github.io/publications/verified-testing-report.pdf))   

### 2013    
* **Automating inductive proofs using theory exploration** (CADE 2013)       
  Koen Claessen, Moa Johansson, Dan Rosén, Nicholas Smallbone        
  ([doi](https://link.springer.com/chapter/10.1007/978-3-642-38574-2_27))      

* **Splittable Pseudorandom Number Generators using Cryptographic Hashing** (Haskell 2013)      
  Koen Claessen, Michał H. Pałka       
  ([doi](https://doi.org/10.1145/2503778.2503784))       

* **Applying random testing to a base type environment (Experience Report)** (ICFP 2013)            
  Vincent St-Amour, Neil Toronto          
  ([doi](https://doi.org/10.1145/2544174.2500616))

* **Testing Blocking Operations with QuickCheck’s Component Library** (Erlang 2013)       
  Ulf Norell, Hans Svensson, Thomas Arts         
  ([doi](https://doi.org/10.1145/2505305.2505310))       

### 2012 
* **Shrinking and showing functions (Functional Pearl)** (Haskell 2012)       
  Koen Claessen           
  ([doi](https://doi.org/10.1145/2430532.2364516))       

* **Feat: functional enumeration of algebraic types** (Haskell 2012)           
  Jonas Duregård, Patrik Jansson, Meng Wang      
  ([doi](https://doi.org/10.1145/2364506.2364515))            
  
* **Testing type class laws** (Haskell 2012)        
  Johan Jeuring, Patrik Jansson, Cláudio Amaral        
  ([doi](https://doi.org/10.1145/2364506.2364514))

* **Advances in Lazy SmallCheck** (IFL 2012)         
  Jason S. Reich, Matthew Naylor, Colin Runciman         
  ([doi](https://doi.org/10.1007/978-3-642-41582-1_4))

* **Boosting the Permissiveness of Dynamic Information-Flow Tracking by Testing** (ESORICS 2012)        
  Arnar Birgisson, Daniel Hedin, Andrei Sabelfeld              
  ([doi](https://doi.org/10.1007/978-3-642-33167-1_4))    

### 2011  
* **Testing an Optimising Compiler by Generating Random Lambda Terms** (AST 2011)         
  Michał H. Pałka, Koen Claessen, Alejandro Russo, John Hughes         
  ([doi](https://dl.acm.org/doi/10.1145/1982595.1982615))      

### 2010 
* **Testing Polymorphic Properties** (ESOP 2010)          
  Jean-Philippe Bernardy, Patrik Jansson, Koen Claessen           
  ([doi](https://link.springer.com/chapter/10.1007/978-3-642-11957-6_8))        

* **QuickSpec: Guessing Formal Specifications Using Testing** (TAP 2010)      
  Koen Claessen, Nicholas Smallbone, John Hughes          
  ([doi](https://doi.org/10.1007/978-3-642-13977-2_3))

* **Using Temporal Relations to Specify and Test an Instant Messaging Server** (AST 2010)           
  John Hughes, Ulf Norrell, Jérôme Sautret       
  ([doi](https://doi.org/10.1145/1808266.1808281))

### 2008 
* **SmallCheck and Lazy SmallCheck: automatic exhaustive testing for small values** (Haskell 2008)    
  Colin Runciman, Matthew Naylor, Fredrik Lindblad   
  ([doi](https://doi.org/10.1145/1411286.1411292))       

* **EasyCheck: test data for free** (FLOPS 2008)           
  Jan Christiansen, Sebastian Fischer         
  ([doi](https://doi.org/10.1007/978-3-540-78969-7_23))            

### 2007
* **Systematic generation of glass-box test cases for functional logic programs** (PPDP 2007)       
  Sebastian Fischer, Herbert Kuchen     
  ([doi](https://doi.org/10.1145/1273920.1273930))      

### 2006     
* **Testing Properties of Generic Functions** (IFL 2006)         
  Patrik Jansson, Johan Jeuring, Laurence Cabenda, Gerbo Engels, Jacob Kleerekoper, Sander Mak, Michiel Overeem, Kees Visser      
  ([doi](https://link.springer.com/chapter/10.1007/978-3-540-74130-5_13))        

### 2002    
* **Testing Monadic Code with QuickCheck** (Haskell 2002)     
  Koen Claessen, John Hughes      
  ([doi](https://doi.org/10.1145/581690.581696))                  

### 2000 
* **QuickCheck: a lightweight tool for random testing of Haskell programs** (ICFP 2000)        
  Koen Claessen, John Hughes      
  ([doi](https://dl.acm.org/doi/10.1145/351240.351266))

***

## Dissertations 
## 2024 
- **Property-Based Testing for the People** (PhD Dissertation, University of Pennsylvania)        
  Harrison Goldstein          
  ([pdf](https://harrisongoldste.in/papers/dissertation.pdf))

## 2022 
- **Testing by Dualization** (PhD Dissertation, University of Pennsylvania)            
  Yishuai Li       
  ([pdf](https://arxiv.org/pdf/2210.01047))          

## 2020 
- **Efficiency Three Ways: Tested, Verified, and Formalised** (PhD Dissertation, University of Nottingham)      
  Martin Handley       
  ([pdf](https://people.cs.nott.ac.uk/pszgmh/handley-thesis.pdf))

## 2019 
- **Narrowing in on Property-Based Testing** (PhD Dissertation, University of Nottingham)          
  Jonathan Fowler           
  ([pdf](http://eprints.nottingham.ac.uk/57009/1/thesis.pdf))

## 2018 
- **Random Testing for Language Design** (PhD Dissertation, University of Pennsylvania)         
  Leonidas Lampropoulos       
  ([pdf](https://lemonidas.github.io/pdf/Leo-PhD-Thesis.pdf))            

## 2017      
- **Tools for Discovery, Refinement and Generalization of Functional Properties by Enumerative Testing** (PhD Dissertation, University of York)          
  Rudy Braquehais             
  ([pdf](https://matela.com.br/thesis-rudy.pdf))

## 2013     
- **Lightweight Verification of Functional Programs** (PhD Dissertation, Chalmers University of Technology)       
  Nicholas Smallbone       
  ([pdf](https://smallbone.se/papers/thesis.pdf))        
