#math

# Deep dive: Quaternions H\mathbb{H}

## 1) Definition & multiplication

A quaternion is

q=a+bi+cj+dk(a,b,c,d∈R)q=a+bi+cj+dk \quad (a,b,c,d\in\mathbb{R})

with basis units i,j,ki,j,k obeying

i2=j2=k2=ijk=−1,ij=k,  jk=i,  ki=j,i^2=j^2=k^2=ijk=-1,\quad ij=k,\; jk=i,\; ki=j,

and **non-commutativity** (e.g., ji=−kji=-k, etc.). Conjugate, norm, and inverse:

q∗=a−bi−cj−dk,∥q∥=a2+b2+c2+d2,q−1=q∗∥q∥2 (q≠0).q^*=a-bi-cj-dk,\quad \lVert q\rVert=\sqrt{a^2+b^2+c^2+d^2},\quad q^{-1}=\frac{q^*}{\lVert q\rVert^2}\ (q\neq 0).

Product in scalar–vector form: write q=(w,v)q=(w,\mathbf{v}), p=(s,u)p=(s,\mathbf{u}) with v,u∈R3\mathbf{v},\mathbf{u}\in\mathbb{R}^3:

q⊗p=(ws−v ⁣⋅ ⁣u,    wu+sv+v ⁣× ⁣u).q\otimes p=\big(ws-\mathbf{v}\!\cdot\!\mathbf{u},\;\; w\mathbf{u}+s\mathbf{v}+\mathbf{v}\!\times\!\mathbf{u}\big).

Key properties: associative, distributive, not commutative; multiplicative norm ∥pq∥=∥p∥∥q∥\lVert pq\rVert=\lVert p\rVert\lVert q\rVert; H\mathbb{H} is a real **division algebra**.

---

## 2) Unit quaternions and 3D rotations

Unit quaternions (∥q∥=1\lVert q\rVert=1) live on S3S^3 and encode 3D rotations.

- **Axis–angle →\to quaternion.** For rotation by θ\theta about unit axis u\mathbf{u}:
    

q=(cos⁡θ2, usin⁡θ2)=(w,x,y,z).q=\Big(\cos\tfrac{\theta}{2},\ \mathbf{u}\sin\tfrac{\theta}{2}\Big)=(w,x,y,z).

- **Rotate a vector.** Treat v∈R3\mathbf{v}\in\mathbb{R}^3 as a “pure” quaternion (0,v)(0,\mathbf{v}):
    

v′=q (0,v) q−1.\mathbf{v}' = q\,(0,\mathbf{v})\,q^{-1}.

- **Double cover.** qq and −q-q represent the **same** spatial rotation (so pick a sign convention and stick to it).
    

### Rotation matrix from a unit quaternion q=(w,x,y,z)q=(w,x,y,z)

R(q)=[1−2(y2+z2)2(xy−wz)2(xz+wy)2(xy+wz)1−2(x2+z2)2(yz−wx)2(xz−wy)2(yz+wx)1−2(x2+y2)].R(q)=\begin{bmatrix} 1-2(y^2+z^2) & 2(xy-wz) & 2(xz+wy)\\ 2(xy+wz) & 1-2(x^2+z^2) & 2(yz-wx)\\ 2(xz-wy) & 2(yz+wx) & 1-2(x^2+y^2) \end{bmatrix}.

### Worked example

Rotate (1,0,0)(1,0,0) by 90∘90^\circ about +z^+\hat z.  
q=(cos⁡45∘,0,0,sin⁡45∘)=(22,0,0,22)q=(\cos 45^\circ,0,0,\sin 45^\circ)=(\tfrac{\sqrt2}{2},0,0,\tfrac{\sqrt2}{2}).  
Then v′=q(0,(1,0,0))q−1=(0,1,0) \mathbf{v}'=q(0,(1,0,0))q^{-1}=(0,1,0).

---

## 3) Lie-group viewpoint

- The unit quaternions form the Lie group SU(2)SU(2) (topologically S3S^3).
    
- The map q↦R(q)q\mapsto R(q) is a 2-to-1 surjective homomorphism SU(2)→SO(3)SU(2)\to SO(3).
    
- Composition of rotations = quaternion multiplication (watch left vs right action conventions).
    

---

## 4) Exponential, logarithm, and “polar form”

Write q=(w,v)q=(w,\mathbf{v}), v=∥v∥v=\lVert\mathbf{v}\rVert, v^=v/v\hat{\mathbf{v}}=\mathbf{v}/v (if v≠0v\neq0).

- **Exponential**
    

exp⁡(q)=ew(cos⁡v, v^sin⁡v).\exp(q)=e^{w}\Big(\cos v,\ \hat{\mathbf{v}}\sin v\Big).

- **Logarithm** (for q≠0q\neq 0, with ϕ=arccos⁡ ⁣(w/∥q∥)\phi=\arccos\!\big(w/\lVert q\rVert\big))
    

log⁡(q)=(ln⁡∥q∥, v^ ϕ).\log(q)=\big(\ln\lVert q\rVert,\ \hat{\mathbf{v}}\ \phi\big).

For a unit quaternion, log⁡(q)=(0, v^ θ2)\log(q)=(0,\ \hat{\mathbf{v}}\ \tfrac{\theta}{2}), where θ=2arctan⁡2(∥v∥,w)\theta=2\arctan2(\lVert\mathbf{v}\rVert,w).

- **Axis–angle “polar form”**  
    q=∥q∥ exp⁡(0, u^ θ2).q=\lVert q\rVert\,\exp\big(0,\ \hat{\mathbf{u}}\,\tfrac{\theta}{2}\big).
    

---

## 5) Interpolation & averaging

- **SLERP** between unit q0,q1q_0,q_1 (ensure shortest path: if ⟨q0,q1⟩<0\langle q_0,q_1\rangle<0, replace q1←−q1q_1\leftarrow -q_1):
    

slerp(q0,q1;t)=sin⁡((1−t)Ω)sin⁡Ω q0+sin⁡(tΩ)sin⁡Ω q1,Ω=arccos⁡(⟨q0,q1⟩).\mathrm{slerp}(q_0,q_1;t) =\frac{\sin((1-t)\Omega)}{\sin\Omega}\,q_0+\frac{\sin(t\Omega)}{\sin\Omega}\,q_1, \quad \Omega=\arccos\big(\langle q_0,q_1\rangle\big).

- **Squad / geodesic splines** use quaternion logs/exps on S3S^3 for smooth camera/robot motion.
    

---

## 6) Kinematics with angular velocity

Let ω∈R3\boldsymbol{\omega}\in\mathbb{R}^3 be angular velocity. Two common conventions (pick one consistently):

- **Body-frame right multiplication**:
    

q˙=12 q⊗(0,ω).\dot q=\tfrac{1}{2}\,q\otimes(0,\boldsymbol{\omega}).

- **World-frame left multiplication**:
    

q˙=12 (0,ω)⊗q.\dot q=\tfrac{1}{2}\,(0,\boldsymbol{\omega})\otimes q.

Matrix form (for the first convention):

q˙=12[0−ωx−ωy−ωzωx0  ωz−ωyωy−ωz0  ωxωz  ωy−ωx0]q.\dot q=\tfrac{1}{2} \begin{bmatrix} 0&-\omega_x&-\omega_y&-\omega_z\\ \omega_x&0&\ \ \omega_z&-\omega_y\\ \omega_y&-\omega_z&0&\ \ \omega_x\\ \omega_z&\ \ \omega_y&-\omega_x&0 \end{bmatrix}q.

**Integration tips:** renormalize qq after each step; for higher accuracy use exp⁡(12(0,ωΔt))⊗q\exp\big(\tfrac{1}{2}(0,\boldsymbol{\omega}\Delta t)\big)\otimes q.

---

## 7) Conversions

- **Quaternion ↔\leftrightarrow axis–angle**
    
    - From axis–angle: q=(cos⁡θ2, usin⁡θ2)q=(\cos\frac\theta2,\ \mathbf{u}\sin\frac\theta2).
        
    - From unit q=(w,v)q=(w,\mathbf{v}): θ=2arctan⁡2(∥v∥,w)\theta=2\arctan2(\lVert\mathbf{v}\rVert,w), u=v/sin⁡(θ/2)\mathbf{u}=\mathbf{v}/\sin(\theta/2) if θ≠0\theta\neq 0.
        
- **Quaternion ↔\leftrightarrow Euler angles**: depends on order (e.g., ZYX); always specify convention to avoid sign/order mistakes.
    
- **Quaternion ↔\leftrightarrow rotation matrix**: use R(q)R(q) above; from RR, extract w=121+tr⁡Rw=\tfrac{1}{2}\sqrt{1+\operatorname{tr}R} (watch numerical stability near singular cases).
    

---

## 8) Matrix & complex representations

Embed H\mathbb{H} in 2×22\times2 complex matrices using Pauli-like basis:

a+bi+cj+dk ↦ [a+bic+di−c+dia−bi].a+bi+cj+dk \ \mapsto\ \begin{bmatrix} a+bi & c+di\\ -c+di & a-bi \end{bmatrix}.

This makes associativity obvious and connects to SU(2)SU(2) and spinors.

---

## 9) Geometry & topology highlights

- Pure imaginary quaternions Im H≅R3\mathrm{Im}\,\mathbb{H}\cong\mathbb{R}^3.
    
- Conjugation action v↦qvq−1v\mapsto qvq^{-1} preserves dot and cross products → genuine rotations.
    
- **Hopf fibration:** the map S3→S2S^3\to S^2 arises naturally from unit quaternions, underpinning the double cover SU(2)→SO(3)SU(2)\to SO(3).
    

---

## 10) Common pitfalls & best practices

- **Sign ambiguity:** qq and −q-q are the same rotation—flip consistently (e.g., enforce ⟨qk+1,qk⟩≥0\langle q_{k+1},q_k\rangle\ge 0).
    
- **Normalization drift:** floating-point ops cause ∥q∥≠1 \lVert q\rVert\neq 1; renormalize.
    
- **Order matters:** pq≠qppq\neq qp. Know whether your system applies rotations as **pre** or **post** multiplications.
    
- **Euler conversion traps:** always specify axis order (XYZ, ZYX, etc.) and intrinsic/extrinsic meaning.
    
- **Small-angle numerics:** use series/`atan2` for stable axis extraction when θ→0\theta\to 0.
    

---

## 11) Beyond quaternions

- **Octonions O\mathbb{O} (8D):** lose associativity (but alternative).
    
- **Cayley–Dickson:** 16D sedenions and beyond (introduce zero divisors).
    
- **Clifford (geometric) algebras:** any dimension, associative, extremely useful—but not division algebras.
    

---

## 12) Minimal “toolbox” you can memorize

- q=(cos⁡θ2, usin⁡θ2)q=(\cos\frac\theta2,\ \mathbf{u}\sin\frac\theta2), rotate via v′ ⁣= ⁣qvq−1v'\!=\!qvq^{-1}.
    
- Multiply: (w,v)⊗(s,u)=(ws−v ⁣⋅ ⁣u, wu+sv+v ⁣× ⁣u)(w,\mathbf{v})\otimes(s,\mathbf{u})=(ws-\mathbf{v}\!\cdot\!\mathbf{u},\ w\mathbf{u}+s\mathbf{v}+\mathbf{v}\!\times\!\mathbf{u}).
    
- Inverse (unit): q−1=q∗=(w,−v)q^{-1}=q^*=(w,-\mathbf{v}).
    
- SLERP with dot-product sign fix; renormalize often.
    

If you want, I can turn this into an Obsidian-ready note (with more LaTeX and a few derivations), or add Python utilities for converting and composing rotations.