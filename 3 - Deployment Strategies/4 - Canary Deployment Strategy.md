# Canary Deployment Strategy

Canary deployment ek progressive deployment strategy hai jisme application ka naya version kuch percent users ke liye deploy kiya jata hai.

Isme Blue-Green strategy jesa server setup hota hai, jaise 1 server par purana application chal rha hota hai aur dusre server par naya application deploy kiya jata hai aur uspe kuch users ko hi access diya jata hai.

“Canary deployment ek aisi strategy hai jisme hum naya version sabko ek saath nahi dete, balki pehle sirf thode users ko dete hain, fir dheere-dheere sabko.”

Matlab tum app ke naye version ko deploy karte ho, lekin initially sirf 1%, 5%, ya 10% traffic us version par jaata hai. Agar sab kuch theek chalta hai, to gradually 100% traffic nayi version par divert kar dete ho.

<br>

<img src="https://drive.google.com/uc?export=view&id=1-YbBLPH4Y-CZta0edBxKGNCAS4HfA2VI" width="450" height="260">

<br>

<img src="https://drive.google.com/uc?export=view&id=1gWGXY2muPRIpf14h-slZ9gFR0hOa37go" width="450" height="260">

<br>

<img src="https://drive.google.com/uc?export=view&id=109KQoct41nTh5ayHQgCnrSOQbQSFhGZZ" width="450" height="260">
