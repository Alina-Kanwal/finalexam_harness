# finalexam_harness
Teeno ek hi cheez hain: harness (loop + tools + permissions). Farq sirf ye hai kis ne banaya aur andar model fix hai ya badalne layak.
Model = sirf intelligence. Ye baat kar sakta hai, sochsakta hai, jawab de sakta hai — lekin akela ye kuch kar nahi sakta. Na file khol sakta hai, na command chala sakta hai, na khud ko rok sakta hai.
Agent = Model + Harness. Model ke gird jo box hai (loop, tools, permissions, checks) — wahi is dimagh ko haath-paar deta hai, aur usko reliable banata hai.
************************4 Parts of harness
(1) chota loop jo model ko kaam pe rakhta hai, (2) tools jo woh use kar sakta hai, (3) context management (kya usko yaad rehta hai, kya bhool jata hai), (4) control (kya usko rokta hai).
**************************Do halves: 
Upar wali picture dekho — inner harness woh hissa hai jo model banane wali company (jaise Anthropic) khud banati hai: tool calling, context window size. Ye tum edit nahi kar sakti, sirf model choose kar ke select karti ho. Outer harness woh hissa hai jo tum khud set karti ho: permissions, hooks, checks, logs. Jab kabhi agent kuch ghalat kare, sabse pehla sawal ye hoga — masla inner mein hai (to model change karo) ya outer mein hai (to rule likho)?
*************************Paanch verbs: 
Poora course inhi paanch kaamon ke gird ghoomta hai — constrain (kya karne ki ijazat nahi), inform (kya jaanna zaroori hai), verify (kaam sahi hai ya nahi), correct (ghalti theek karo, phir dobara na ho), escalate (jab harness faisla na kar sake, insaan ko bhejo). Ek zaroori rule yaad rakhna: guardrail hamesha harness mein hota hai, prompt mein nahi — "please .env mat kholna" sirf ek request hai, ek deny rule ek deewar hai.
*************************
bas Claude Code aur Gemini CLI mein model fix hai (uski company ne bana ke die), jabke OpenCode mein wahi harness generic hai aur tum khud model plug karti ho.



