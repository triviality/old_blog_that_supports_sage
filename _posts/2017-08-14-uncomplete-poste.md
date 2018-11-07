#برهان المتباينة Inequality proof
في كتاب سلفرمان، الوضع كالتالي ليكنن ${\bf{s}} = c_1c_2c_3\dots c_n$ نصا يحوي على *n* من الأحرف وليكن $F_i$ تكرار الحرف   *i*  في النص  $\bf{s}$ ولنعرف أيضا   $IndCon({\bf{s}}) := \frac{1}{n\left(n-1\right)} \sum_{i=0}^{25} F_i \left(F_i -1\right)$.

الهجوم هو نفسه في حال كانت الأحرف عددها 32 أو 25 أو n (حيث $n>1$( لذا سنتعامل مع الحال العامة. ما نود إثباته هو التالي

$$\begin{alignat}{2}
\frac{1}{n\left(n-1\right)} &\sum_{i=0}^{\color{red}k} F_i \left(F_i -1\right) &&\geq \frac{1}{n\left(n-1\right)} \sum_{i=0}^{\color{red}k} \frac{n}{k} \left(\frac{n}{k} -1\right) =  \frac{k}{n\left(n-1\right)}  \frac{n}{k} \left(\frac{n}{k} -1\right) \\
 \Leftrightarrow
 &\sum_{i=0}^{\color{red}k} F_i \left(F_i -1\right) &&\geq  k \frac{n}{k} \left(\frac{n}{k} -1\right) 
\end{alignat}$$ 

الجانب الأيسر من المتباينة هو أي توزيع distribution، بينما الجانب الأيمن هو التوزيع المنتظم the unifrom distribuation حيث يظهر كل حرف بنفس الاحتمال 

## تقريبا
لنتأكد من أننا على المسار الصحيح لندرس الحالات المتطرفة، فمثلا لو كان النص كله من نفس الحرف (مثلا أأأأأأأأأ) أي $F_1 = n$ و $F_i = 0,\ i \not = 1$  t تصبح المتباينة في الأعلى $n\left(n-1\right) \geq  k \frac{n}{k} \left(\frac{n}{k} -1\right)$.

بما أن $n \approx  n-1$ فإن المتباينة تصبح $n^2 \geq  k \frac{n^2}{k^2} \Leftrightarrow 1 \geq  \frac{k}{k^2}  = \frac{1}{k} \Leftrightarrow k\cdot 1 \geq 1  $



## برهان 

### حرفين (نفترض معرفتك بحساب التفاصل)

لو وجدنا بان القيمة القصوى للدالة   $f\left(F_1, F_2\right) = F_1\left(F_1 -1\right) + F_2\left(F_2 -1\right)$, حيث $F_1 + F_2 = n{\bf,} \ F_i  \in [1, n]$ عند  $F_1 = F_2 = \frac{n}{2}$ فسنكون قد أثبتنا ما نريده.
 
بما أن $F_1 + F_2 = n$  فإننا نقدر على التعامل مع $f$ على أنها دالة بمتغير واحد أي $f(F_1) =   F_1\left(F_1 -1\right)  + \left(n- F_1\right)  \left(n - F_1 -1\right)$ ، ﻷنه من الأسهل التعامل بمتغير واحد بدلا من فوضى المتغيرين.

من حساب التفاضل نعلم أن القيم القصوى )العظمى والضغرى) للدالة $f$ هي إما عند النقط الحرجة critical points (عندما تكون المشتقة تساوي صفر) أو عند الحدود.
#### إيجاد النقاط الحرجة 
$\frac{d}{dF_1} f = F_1 + F_1 -1 -\left(n- F_1 -1\right) - \left(n- F_1 \right)  = 4F_1 - 2n \Rightarrow 0 =  \frac{d}{dF_1} f \Leftrightarrow   4F_1 -2n = 0 \Leftrightarrow F_1 = \frac{n}{2}$. Thus,there is, only, one critical poitn $F_1 = \frac{n}{2}$


#### حساب القيم العظمى والصغرى

|  $F_1$ |   0	|  $\frac{n}{2}$ 	|   n	| 
|---	|---	|---	|---	|---	|
|   $f(F_1)$	|   $n\left(n-1\right)$	|   $2\cdot \frac{n}{2}\left(\frac{n-2}{2}\right)$	|   $n\left(n-1\right)$	|   	


كما نرى في الجدول التالي بأن $2\cdot \frac{n}{2}\left(\frac{n-2}{2}\right)$ عندما $F_1 = F_2  = \frac{n}{2} $ وهو المطلوب!

[1]:https://en.wikipedia.org/wiki/Critical_point_(mathematics)#Critical_point_of_a_single_variable_function


### ثلاث أحرف أو أكثر (حساب التفاضل والتكامل بعدة متغيرات)


لتكن  $f\left(F_1, F_2, \dots , F_k\right) = \sum_i  F_i\left(F_i -1\right) $ subjects to $ g\left(F_1, F_2, \dots, F_k\right) = \left(\sum_i F_i\right) -n $، ولنبحث عن القيم العظمى أو الصغرى للدالة $f$ وبحالة شبيهة عند المتغيرين 

We are interested in finding  points that maximize/minimize  $f$. As in the case `Two letters`, if we show that the minimum occurs at $F_1 = F_2 = \dots = F_k$ then we are done.




**Recall:** The method of Lagrange multipliers find the maximum/minimum when the gradient of a specific function equals to zero. Below a break down of the  of the `method` which can be implemented in `SageMath`

1. Compute $ \nabla \cdot \left( f-\lambda g\right) = \frac{\partial}{\partial F_1}\left( f-\lambda g\right) dF_1 + \frac{\partial}{\partial F_2}\left( f-\lambda g\right) dF_2 + \dots + \frac{\partial}{\partial F_k}\left( f-\lambda g\right) dF_k   $ where $\lambda$ is an extra variable.
2. Since $dF_i$s are linearly independent then $\nabla \cdot \left( f-\lambda g\right) = 0 \Leftrightarrow \frac{\partial}{\partial F_1}\left( f-\lambda g\right)  = 0  $ for each $i$
3. Solve these equations to find the critical points
4. Substitute each critical point $\left(x_1, x_2, \dots, x_k\right)$ in $f$ i.e. compute $f\left(x_1, x_2, \dots, x_k\right)$ then record it in some set, say $M$. The maximum or minimum value of $f$ subjects to the constraint $g$ must be in $M$ 



**1:** لكل متغير
 $F_1, F_2, \dots, F_k$ لدينا $\frac{\partial}{\partial F_i} f = F_i + F_i -1  = 2F_i -1$ و $\frac{\partial}{\partial F_i} \lambda \cdot g = \lambda$ 

**2:**
$$\begin{alignat}{2}
 &2F_1-1 - \lambda  = 0 \Rightarrow F_1 = \frac{1+\lambda}{2}\\
 &2F_2-1 - \lambda  = 0 \Rightarrow F_2 = \frac{1+\lambda}{2}\\
 &\vdots \\
 &2F_k-1 - \lambda  = 0 \Rightarrow F_k = \frac{1+\lambda}{2}\\
 & \sum_i F_i = n
\end{alignat}$$

**3:**
Hence, $F_i = F_j$ for all $i, j$.  . If we find the value of $F_1$ then we found all $F_i$s value, since they are equal to each other.  Using the last result, $\sum_i F_i = F_1 \sum_i 1 = n   \Rightarrow  F_1= F_2= \dots= F_k = \frac{n}{k}$

**4:**
 $f\left(\frac{n}{k},\frac{n}{k}, \dots, \frac{n}{k}\right) =  k \frac{n}{k} \left(\frac{n}{k} -1\right)  = n \left(\frac{n}{k} -1\right)  $. We know that $ n \left(\frac{n}{k} -1\right)$ is an extreme value, but we don't know whether it is a minimum value or not.  Let us plug in $f$ with any other point. For instance, $F_1 = n, F_i = 0, i\not = j$ then it is clear that $n\left(n-1\right) \geq  n \left(\frac{n}{k} -1\right) \Leftrightarrow   n\geq  \frac{n}{k}$ since it is an extreme point then it must be the minimum of $f$ under the constraint  $g$

 $ n \left(\frac{n}{k} -1\right)$ occurs at $F_1 = F_2 = \dots = F_k$ `Q.E.D`
