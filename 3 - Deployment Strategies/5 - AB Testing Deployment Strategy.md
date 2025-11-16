# A/B Testing Deployment Strategy

<br>

### A/B Testing Hota Kya Hai?

A/B testing ek experimental deployment strategy hai jisme tum do (ya zyada) versions of the same application ko different user groups ke saamne deploy karte ho — aur fir monitor karte ho kaunsa version better perform karta hai.

Ye deployment strategy Canary Strategy pe based hai, matlab Canary jesi hi hai, jaise Canary deployment strategy mein do environments hote hain, kuch users ko naye version pe traffic route karte hain testing ke liye, 

Ese hi A/B Testing strategy mein do alag environments hote hain aur kuch user ko application ke naye environments par traffic route karte hain, lekin A/B testing application ke purane version aur naye version ke beech mein comparision kiya jata hai.

Aur jo version accha perform karta hai usko final choose kiya jata hai aur 100% traffic bheja jata hai.

Canary aur A/B testing mein ye hi difference hai ki A/B strategy mein version ka comparision kiya jata hai.

<br>

<img src="https://drive.google.com/uc?export=view&id=1LPJP0aGHN3vWPPgNFTTKstk1sQBAejDf" width="550" height="560">

<br>

A/B testing ek strategy hai jisme tum users ko randomly group karke unhe alag-alag version of application serve karte ho, taaki measure kar sako kaunsa version zyada effective hai.

**A ka matlab — Old version (current stable**).

**B ka matlab — New version (experiment)**.

Example:
- A = old checkout page.
- B = new checkout page with different button design.

Agar B version se conversion rate badh gaya, to B ko promote kar dete hain production main.

