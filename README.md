# finalexam_harness
Teeno ek hi cheez hain: harness (loop + tools + permissions). Farq sirf ye hai kis ne banaya aur andar model fix hai ya badalne layak.
Model = sirf intelligence. Ye baat kar sakta hai, sochsakta hai, jawab de sakta hai — lekin akela ye kuch kar nahi sakta. Na file khol sakta hai, na command chala sakta hai, na khud ko rok sakta hai.
Agent = Model + Harness. Model ke gird jo box hai (loop, tools, permissions, checks) — wahi is dimagh ko haath-paar deta hai, aur usko reliable banata hai.
model fool ho sakta hai aur galat kadam try kar sakta hai → harness us kadam ko execute hone se pehle pakad leta hai. Harness soch nahi raha, sirf har action pe deewar khara hai.
************************4 Parts of harness
(1) chota loop jo model ko kaam pe rakhta hai, (2) tools jo woh use kar sakta hai, (3) context management (kya usko yaad rehta hai, kya bhool jata hai), (4) control (kya usko rokta hai).
**************************Do halves: 
Upar wali picture dekho — inner harness woh hissa hai jo model banane wali company (jaise Anthropic) khud banati hai: tool calling, context window size. Ye tum edit nahi kar sakti, sirf model choose kar ke select karti ho. Outer harness woh hissa hai jo tum khud set karti ho: permissions, hooks, checks, logs. Jab kabhi agent kuch ghalat kare, sabse pehla sawal ye hoga — masla inner mein hai (to model change karo) ya outer mein hai (to rule likho)?
*************************Paanch verbs: 
Poora course inhi paanch kaamon ke gird ghoomta hai — constrain (kya karne ki ijazat nahi), inform (kya jaanna zaroori hai), verify (kaam sahi hai ya nahi), correct (ghalti theek karo, phir dobara na ho), escalate (jab harness faisla na kar sake, insaan ko bhejo). Ek zaroori rule yaad rakhna: guardrail hamesha harness mein hota hai, prompt mein nahi — "please .env mat kholna" sirf ek request hai, ek deny rule ek deewar hai.
*************************
bas Claude Code aur Gemini CLI mein model fix hai (uski company ne bana ke die), jabke OpenCode mein wahi harness generic hai aur tum khud model plug karti ho.
////////////////////////////////////////////Permission Rules — Allow, Ask, Deny
Raat 3 baje agent koi command chalana chahta hai. Koi jaaga nahi hota check karne ke liye — isliye pehle se likha hua rule faisla karta hai. Har mature harness teen jawabon mein kaam karta hai:
Allow — chup-chap chal jaye
Ask — ruk jaye, insaan se haan lo
Deny — kabhi nahi, chahe koi bhi maange
Rules ko human set krty hain
Ye jawab tum khud dete ho, pehle se, likh kar — settings file mein (settings.json ya opencode.json). Harness khud koi faisla nahi leta ke "allow" ya "deny" — woh sirf tumhara likha hua rule padhta hai.
Kaise select hota hai: Har rule ek action ke pattern se match karta hai. Jab agent koi action karna chahta hai (jaise git push origin claude/fix), harness check karta hai — "iska naam/pattern meri list mein kahin match karta hai?" Agar Bash(git push origin claude/*) allow list mein hai, to allow chalega. Agar koi rule match na kare, default fallback hota hai (aksar ask).
Ek zaroori tarteeb: deny hamesha jeetta hai, phir ask, phir allow. Matlab agar ek broad allow rule hai lekin ek narrow deny rule bhi kisi cheez ko cover karta hai — deny wins, chahe allow list mein bhi likha ho.
///////////////////////////////////////////////////////////////////Sandboxes: Nuksaan ko impossible bana do (Concept 5)
Permission rules = kya karna allowed hai (ye action haan ya na)
Sandbox = kis dayre (boundary) ke andar rehkar karna hai — sirf directory nahi, balke filesystem + network + branch teeno ka dayra.
Tumhe pehla sandbox pehle se pata hai — worktree (pichle course se): har run ki apni copy hoti hai project folder ki, taake kuch bhi tumhari asal copy ko na chhue. 
Permission rules = kya karna allowed hai (ye action haan ya na)
Sandbox = kis dayre (boundary) ke andar rehkar karna hai — sirf directory nahi, balke filesystem + network + branch teeno ka dayra
Harness iske gird teen aur deewarein lagata hai:
Filesystem fence — agent sirf apne workspace mein likh sakta hai, kahin aur nahi. Home directory, doosre projects, system files — sirf mana nahi, pahunch se bahar.
Network fence — unattended runs ko chand allowed domains milte hain, ya bilkul network nahi. Agar internet hi na ho, to koi injected instruction bhi code leak nahi kar sakti.
Branch fence — unattended pushes sirf claude/ branches pe jaate hain, isliye main insaan ke gate ke peeche rehta hai — politely nahi, structurally.
Harness = poora box (loop + tools + context + control) — bada umbrella term.
Sandbox = harness ka ek hissa, khaas taur pe "Constrain" wala verb — woh deewarein jo limit karti hain agent kahan kaam kar sakta hai.
Shayad "harness" aur "sandbox" mix ho gaye — farq yaad dilata hoon:
Harness = poora box (loop + tools + context + control) — bada umbrella term.
Sandbox = harness ka ek hissa, khaas taur pe "Constrain" wala verb — woh deewarein jo limit karti hain agent kahan kaam kar sakta hai.
Misal: rule kehta hai "agent files edit kar sakta hai" (kya — allowed). Sandbox kehta hai "sirf apne workspace folder ke andar" (kahan — limit). Agar agent fool ho kar kisi doosri jagah edit karne ki koshish kare, rule usay rokta nahi (kyunke "edit karna" to allowed hai) — lekin sandbox rok deta hai, kyunke woh jagah uski hadd se bahar hai.
//////////////////////////////////////////////////////////////////////Inform
Agent ko kaam karne ke liye 3 tarah ki maloomat chahiye, aur har tarah ka apna ghar hai:
1. Rules file — "hamesha sach baatein"
Jaise: "ye project pnpm use karta hai, npm nahi." Ye baat kabhi nahi badalti, isliye ye ek file mein likh di jati hai jo agent har session shuru hone pe khud parh leta hai.
2. Skills — "is khaas kaam ka tareeqa"
Jaise: "har roz subah bugs check karne ka tareeqa." Ye sirf tab load hota hai jab wahi kaam aaye — hamesha nahi, sirf zaroorat par.
3. Connectors — "kahan tak pahunch hai"
Jaise: agent Gmail se connect hai ya nahi, GitHub se connect hai ya nahi. Ye batata hai agent kya-kya chhoo sakta hai.
////////////////////////////////////////////////////////////////////////Concept 7 — AX (Agent Experience)
Jaise ek app UX insaan ke liye design hoti hai, waise hi harness ke andar tools aur errors agent ke liye design hote hain — kyunke kaam ke waqt agent akela hota hai, kisi se pooch nahi sakta. Ye design teen jagah hoti hai:
1. Kam tools rakho, zyada mat do
Har tool ek faisla hai jo agent ko lena hai — "ye sahi tool hai ya wo?" Jitne zyada milte-julte tools honge, utni zyada ghalti ka chance. Agar insaan engineer khud confuse ho ke konsa tool sahi hai, to agent bhi confuse hoga.
2. Tool ka description mukammal ho
Description mein 3 cheezein honi chahiye: (a) kya karta hai, (b) kaise use hota hai, (c) kitna result deta hai. Misal: "Customer ko email ya ID se dhoondta hai. Sirf 20 entries wapas deta hai (poori list nahi)." Agar sirf "customer tool" likha ho, agent ko andaza nahi hoga.
3. Error mein agla kadam ho
Jab tool fail ho, uska text (jo tum code mein khud likhti ho, jaise except block mein) sirf "kya ghalat hua" na bataye — "agla kya karna hai" bhi bataye. "Error 403" beat zaya karta hai. "403: repo scope maango" agla run khud theek kar leta hai — kyunke agent ka agla kadam usi error text se aata hai.
//////////////////////////////////////////////////////////////////Verify & Correct
"Beat" ka matlab hai — agent ka ek pura chakkar/run, ek baar shuru se lekar khatam hone tak.
Beat = usi loop ka ek single run.
Pichle course ka maker-checker sirf beat khatam hone par check karta tha. Hook har waqt check karta hai — ek code jo harness khud, automatically chalata hai, model ki marzi ke bagair. Teen jagah:
Pehle wala hook = jo galtiyan pehle se maloom hain (jaise "is file ko mat chhuo") — unhe rok deta hai
Baad wala hook = jo galtiyan sirf result dekh kar pata chalti hain (jaise code ka lint error) — unhe pakad kar agent ko wapas bhej deta hai, taake khud theek kare
End wala hook = poore kaam ka aakhri saboot — jaise "sab tests pass hue?" — chahe har chhota step allowed tha
Harness = poora system, sab kuch mil ke (loop, tools, permissions, hooks, sab). Bada umbrella.
Hook = harness ka ek chhota tool, jo sirf ek kaam karta hai: automatically check karna, kisi khaas waqt par (pehle, baad, ya end mein).
Pehle course mein "kaam ho gaya" sirf model ka apna daawa hota tha (khud bol deta "Done!"). Hooks isko badal dete hain — ab "done" wo cheez hai jo harness ne saboot ke sath check ki, na ke sirf model ne bola. To agent ka lafz "done" wahi rehta hai — bas ab uska haq milna harness ke saboot (test pass) par depend karta hai, sirf uski apni marzi par nahi.
/////////////////////////////////////////////////////////////////////////////Typed Output
Maan lo checker apni report likh kar deta hai: "Ye mostly theek hai, lekin kuch shak hai..." — ab agent isay kaise samjhe? PASS ya FAIL?
Isliye checker ko ek fixed, sakht shakal mein jawab dena hota hai — jaise:
{"verdict": "PASS", "risk": "low"}
Sirf "PASS" ya "FAIL" — koi aur lafz allowed nahi. Fir code khud check karta hai ke jawab isi sakht shakal mein hai ya nahi.
Agar checker koi ajeeb jawab de (jaise "MAYBE"), to system usay nahi maanta — seedha kisi insaan ke paas bhej deta hai.
/////////////////////////////////////////////////////////////////////Correct (Recovery + Ratchet)
Jab kuch ghalat ho, do alag kaam karne padte hain:
1. Recovery (turant kaam) — is run ko bachana
Agar masla temporary hai (network slow) → dobara try karo
Agar masla permanent hai (permission hi nahi hai) → dobara try mat karo, insaan ko bhejo
Agar agent khud apna kaam kharab kar chuka ho → checkpoint (pichla acha save point, jaise git commit) pe wapas jao
2. Ratchet (hamesha ke liye kaam) — system ko sudharo
Jab bhi koi ghalti hoti hai, sirf usay theek mat karo — harness mein hi aisa fix daal do ke wahi ghalti dobara ho hi na sake. Ratchet ka matlab: ek aisa tool jo sirf ek taraf ghumta hai, wapas nahi jata — har fix permanent ban jata hai.
Kaunsi ghalti kahan fix hoti hai (4 types):
Ghalti	Fix kahan
Agent ko kuch pata nahi tha	rules file / skill (Concept 6)
Agent ne aisa kaam kiya jo allowed hi nahi tha	permission rule / sandbox (Concept 4-5)
Ghalat kaam ko "done" keh diya	hook / typed output (Concept 8-9)
Sahi cheezein, ghalat tarteeb	task ko chota karo, structure badlo

























