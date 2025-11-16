# Blue-Green deployment strategy

<br>

### Blue-Green Deployment Hota Kya Hai?

Blue-Green Deployment ek zero downtime deployment strategy hai jisme hum application ke do alag environments banate hain:
- Blue Environment → Current live version (jo abhi users use kar rahe hain).
- Green Environment → New updated version (jo tum deploy kar rahe ho).

Blue-Green strategy mein 2 envrionments hote hain, ek Blue Environment jispar current application chal rha hota hai aur 100% traffic isi environment par jata hai. Dusra environment hota hai Green, humko application ka naya version green environment par deploy karna hota hai, fir testing karke Blue se Green Environment ko swap kar dete hain matlab blue pe jane wale traffic ko green par bhej dete hain, 100% traffic fir green environment par jata hai.

Isi ko blue green deployment strategy kehte hain.

<img src="https://drive.google.com/uc?export=view&id=1TTsJm_d2TwJTY9b_33uqHtlLcMlXxudA" width="450" height="260">
