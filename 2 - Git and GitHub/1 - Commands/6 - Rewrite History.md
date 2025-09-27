# Rwwrite History

<img src="https://drive.google.com/uc?export=view&id=1YUce2YXbh8LDuXmDtWtAR1ef0m2W5YXI" width="450" height="220">

<br>

### git rebase [branch]

<br>

**Pehle Samajho: Git Rebase kya hai?**:

Git mein humare paas branches hoti hain. Jab alag-alag branches par alag commits bante hain to unke histories parallel chalti hain. 

Normally, agar hume ek branch ke commits doosri branch mein lana ho to hum ```git merge``` karte hain. Isse ek merge commit ban jata hai, aur history thodi complex ho jati hai (zig-zag).

git rebase ek alternative hai merge ka. Ye kya karta hai:
- Tumhari current branch ke commits ko pick karke, ek naye base par replay karta hai.
- Matlab tumhari branch ka base change ho jata hai, jaise ki wo latest main branch se hi nikli ho.

Isse commit history linear ho jati hai (seedhi line me), jo clean aur samajhne me easy hoti hai.

<br>

**Kya karta hai**:
- rebase का मतलब होता है किसी branch के commits को दूसरी branch के ऊपर apply करना।
- Simple भाषा में: मान लो तुमने main branch se ek branch banai feature aur feature branch पर काम किया, और उसी समय किसी और ने main branch पर नए commits कर दिए। अब तुम्हारे commits main से पीछे रह गए। ```git rebase main``` चलाने पर तुम्हारे feature वाले commits ऐसे appear होंगे जैसे कि वो main branch के latest commit के ऊपर बनाए गए हों।

- git rebase command aapke branch ke sabhi commits ko temporarily hata deta hai.
- Uske baad, target branch (jaise main ya master) ke latest commits laata hai.
- Phir aapke commits ko ek ek karke, naye base (target branch) par dubara apply karta hai. Yeh naye commits ke form me hota hai, pura history rewrite hoti hai.

**Example**:

Situation (before rebase):
```
main:    A---B---C
              \
feature:       D---E
```

- ```main``` branch pe commits = A, B, C
- ```feature``` branch pe commits = D, E
- Tumne B commit ke baad ```feature``` branch banayi thi, lekin ab ```main``` mein ek naya commit C aa gaya hai.

Command:
```
git checkout feature
git rebase main
```

Resutl after Rebase:
```
main:    A---B---C
                  \
feature:           D'---E'
```
- Tumhari feature branch ke commits D aur E re-applied hue latest commit C ke baad.

क्यों ज़रूरी है:
- Rebase clean और linear history बनाने के लिए use होता है।
- यह history को आसान बनाता है, जिससे ऐसा लगता है कि सबने sequentially काम किया।
- Dhyaan rahe: Rebase commits ke naye versions banata hai (isliye unke IDs change ho jate hain).

Real life use-case:
- जब team चाहती है कि project history linear दिखे, जैसे:
  - ```main``` branch हमेशा clean रहे।
  - Merge commits avoid करना हो।
- तुम्हारे ```feature``` branch के changes ऐसे दिखें कि वो directly ```main``` के latest changes के बाद आए।

**Interactive Rebase**:

```git rebase -i [base]``` ka matlab hai tum ek interactive mode khologe jahan tum commits ko reorder, squash (combine), edit kar sakte ho.

Example:
```
git checkout feature
git rebase -i main
```

Terminal khulega:
```
pick abc123 Added feature1
pick def456 Added feature2
```

Tum pick ko squash me badal sakte ho agar tum dono commits ko combine karna chahte ho.

<br>
<br>

### git reset --hard [commit]
