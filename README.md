個人訊息:
姓名: 葉揚昀
學號: 108202529
性別: 男
Email: lim299792458ms@g.ncu.edu.tw

教育經歷:
院校:
大二: 中央資管(大一升大二時轉系)
大一: 中央物理
高中: 嘉義高中

成績:
大一上學期: 系排: 3/47		班排: 3/47
大一下學期: 系排: 1/45		班排: 1/45
大二上學期: 系排: 4/107	班排: 1/53
 

程式專案:
我在物理系時雖然程式基本功不太紮實，但在處理實驗物理、力學、電磁學的project、simulation時會應用VB、Python去處理它們。由於我在物理系時寫的程式都只停留在應用層面，因此我轉入資管系學習基礎程式設計的理論。以下我會列舉其中幾個例子來加以說明:

1.用VB模擬雙星運動
由於VB是以物件導向為主的程式，因此我以幾個力學公式(T=2πL^1.5 √(1/(G(M_1+M_2))),r_1=M_2/(M_1+M_2 )×L)、while迴圈和一些array來計算、儲存模擬的data並運用Picture Box、Timer把結果以簡易動畫形式show出來。

以while迴圈把各個時間點的data存入array:
```
       While time(i) <= 10 * T
            time(i + 1) = time(i) + dt
            theta(i + 1) = theta(i) + omega * dt
            M1_r(i + 1, 0) = r1 * Cos(theta(i + 1))
            M1_r(i + 1, 1) = r1 * Sin(theta(i + 1))
            M2_r(i + 1, 0) = -r2 * Cos(theta(i + 1))
            M2_r(i + 1, 1) = -r2 * Sin(theta(i + 1))
            i = i + 1
        End While
```

模擬雙星運動的結果:
 

用Python模擬實驗project(coupled-driven oscillation):
在做實驗專題時，我用迴圈和6個RK4(一種數值方法)(∵有6個變數要考慮)去模擬兩個單擺中間接彈簧外加一顆步徑馬達施加週期性外力的運動狀態。經過不斷嘗試和debug，終於畫出兩個單擺θ_2-θ_1的Lissajous plot。

迴圈內一部分的RK4，並把數值存入array中:
```
i = 0
while t [i] <= 40:
    
    # Calculate x0, theta1,2
    
    # k1
    kx0_1 = k_x0 (v0[i], t[i])
    kv0_1 = k_v0 (r0[i][0], theta1[i], theta2[i], omega1[i], t[i])
    
    ktheta1_1 = k_theta1(omega1[i], t[i])
    ktheta2_1 = k_theta2(omega2[i], t[i])
    komega1_1 = k_omega1(r0[i][0], theta1[i], theta2[i], omega1[i], t[i])
    komega2_1 = k_omega2(r0[i][0], theta1[i], theta2[i], omega2[i], t[i])
    
    #k2
    kx0_2 = k_x0 (v0[i] + dt/2 * kv0_1, t[i] + dt/2)
    kv0_2 = k_v0 (r0[i][0] + dt/2 * kx0_1, theta1[i] + dt/2 * ktheta1_1, theta2[i] + dt/2 * ktheta2_1, omega1[i] + dt/2 * komega1_1, t[i] + dt/2)
        
    ktheta1_2 = k_theta1(omega1[i] + dt/2 * komega1_1, t[i] + dt/2)
    ktheta2_2 = k_theta2(omega2[i] + dt/2 * komega2_1, t[i] + dt/2)
    komega1_2 = k_omega1(r0[i][0] + dt/2 * kx0_1, theta1[i] + dt/2 * ktheta1_1, theta2[i] + dt/2 * ktheta2_1, omega1[i] + dt/2 * komega1_1, t[i] + dt/2)
    komega2_2 = k_omega2(r0[i][0] + dt/2 * kx0_1, theta1[i] + dt/2 * ktheta1_1, theta2[i] + dt/2 * ktheta2_1, omega2[i] + dt/2 * komega2_1, t[i] + dt/2)
    
    #k3
    kx0_3 = k_x0 (v0[i] + dt/2 * kv0_2, t[i] + dt/2)
    kv0_3 = k_v0 (r0[i][0] + dt/2 * kx0_2, theta1[i] + dt/2 * ktheta1_2, theta2[i] + dt/2 * ktheta2_2, omega1[i] + dt/2 * komega1_2, t[i] + dt/2)
    
    ktheta1_3 = k_theta1(omega1[i] + dt/2 * komega1_2, t[i] + dt/2)
    ktheta2_3 = k_theta2(omega2[i] + dt/2 * komega2_2, t[i] + dt/2)
    komega1_3 = k_omega1(r0[i][0] + dt/2 * kx0_2, theta1[i] + dt/2 * ktheta1_2, theta2[i] + dt/2 * ktheta2_2, omega1[i] + dt/2 * komega1_2, t[i] + dt/2)
    komega2_3 = k_omega2(r0[i][0] + dt/2 * kx0_2, theta1[i] + dt/2 * ktheta1_2, theta2[i] + dt/2 * ktheta2_2, omega2[i] + dt/2 * komega2_2, t[i] + dt/2)
    
    #k4
    kx0_4 = k_x0 (v0[i] + dt/2 * kv0_3, t[i] + dt/2)
    kv0_4 = k_v0 (r0[i][0] + dt * kx0_3, theta1[i] + dt * ktheta1_3, theta2[i] + dt * ktheta2_3, omega1[i] + dt * komega1_3, t[i] + dt)
    
    ktheta1_4 = k_theta1(omega1[i] + dt * komega1_3, t[i] + dt)
    ktheta2_4 = k_theta2(omega2[i] + dt * komega2_3, t[i] + dt)
    komega1_4 = k_omega1(r0[i][0] + dt * kx0_3, theta1[i] + dt * ktheta1_3, theta2[i] + dt * ktheta2_3, omega1[i] + dt * komega1_3, t[i] + dt)
    komega2_4 = k_omega2(r0[i][0] + dt * kx0_3, theta1[i] + dt * ktheta1_3, theta2[i] + dt * ktheta2_3, omega2[i] + dt * komega2_3, t[i] + dt)
    
    # Calculate next pt
    
    r0[i+1][0] = r0[i][0] + dt/6 * (kx0_1 + 2 * kx0_2 + 2 * kx0_3 + kx0_4)
    r0[i+1][1] = r0[0][1]
    v0[i+1] = v0[i] + dt/6 * (kv0_1 + 2 * kv0_2 + 2 * kv0_3 + kv0_4)
    
    theta1[i+1] = theta1[i] + dt/6 * (ktheta1_1 + 2 * ktheta1_2 + 2 * ktheta1_3 + ktheta1_4)
    theta2[i+1] = theta2[i] + dt/6 * (ktheta2_1 + 2 * ktheta2_2 + 2 * ktheta2_3 + ktheta2_4)
    omega1[i+1] = omega1[i] + dt/6 * (komega1_1 + 2 * komega1_2 + 2 * komega1_3 + komega1_4)
    omega2[i+1] = omega2[i] + dt/6 * (komega2_1 + 2 * komega2_2 + 2 * komega2_3 + komega2_4)
    
    r1[i+1][0], r1[i+1][1] = r0[i+1][0] + L1 * sin(theta1[i+1]), r0[0][1] - L1 * cos(theta1[i+1])
    r2[i+1][0], r2[i+1][1] = r0[0][0] + Ls0 + L2 * sin(theta2[i+1]), r0[0][1] - L2 * cos(theta2[i+1])
    
    Ls[i+1] = sqrt((r1[i+1][0]-r2[i+1][0]) ** 2 + (r1[i+1][1]-r2[i+1][1]) ** 2)
    KE[i+1] = (1/2 * m1 * (L1 * omega1[i+1]) ** 2 + 1/2 * m2 * (L2 * omega2[i+1]) ** 2)
    PE[i+1] = (m1 * g * r1[i+1][1] + (m2 * g * r2[i+1][1]) + (1/2 * k * (Ls0 - Ls[i+1]) ** 2)) 
    Et[i+1] = KE[i+1] + PE[i+1]
    
    t[i + 1] = t[i] + dt
    i += 1
```
把array中的數值畫成Lissajous plot:
