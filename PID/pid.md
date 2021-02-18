### PID-regeling

parallelle PID, tijdcontinu:  
```latex
 y(t) = e'(t) + \frac{ \int e'(t)dt }{ T_i } + T_d\cdot \frac{ d e'(t) }{ dt }
```
```
e'(t) = genormaliseerde momentane fout e(t)/Xp
Xp    = proportionele bandbreedte
Ti    = integratietijd, moment waarop I-term = P-term bij vaste afwijking
Td    = differentiatietijd, moment waarop P-term = D-term bij eenparig weglopen
```

Alternatieve schrijfwijze, met G = 1/Xp als versterkingsfactor:  
```latex
 y(t) = G\cdot ( e(t) + \frac{ \int e(t)dt }{ T_i } + T_d\cdot \frac{ d e(t) }{ dt } )  
```

S-domein, integreren is delen door s, differentiëren is vermenigvuldigen met s:  
```latex
\frac{ C(s) }{ E(s) } = K_p + \frac{ K_i }{ s } + K_d\cdot s  
```
```
 Kp = G
 Ki = G/Ti
 Kd = G•Td
```

z-domein, 1/s --> Ts/(1-1/z), of s --> (1-1/z)/Ts:  
```latex
\frac{ C(s) }{ E(s) } =  
  K_p    \cdot\frac{ 1-z^{-1}   }{ 1-z^{-1} } +  
  K_i    \cdot\frac{ T_s        }{ 1-z^{-1} } +  
  K_d/T_s\cdot\frac{(1-z^{-1})^2}{ 1-z^{-1} }
```
```latex
  = \frac{ (K_p + K_i\cdot T_s + K_d/T_s) - (K_p + 2K_d/T_s)z^{-1} + (K_d/T_s)z^{-2} }{ 1-z^{-1} }
```
```latex
  = \frac{ K_0 + K_1 z^{-1} + K_2 z^{-2} }{ 1-z^{-1} }  
```

```
 K0 = Kp + Ki + Kd/Ts
 K1 = Kp +     2Kd/Ts
 K2 =           Kd/Ts
```
Tijddiscreet:  
```latex
 C[k] = C[k-1] + k_0\cdot e[k] + k_1\cdot e[k-1] + k_2\cdot e[k-2]  
```
