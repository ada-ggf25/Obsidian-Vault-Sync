#math #calculus #quantitative_finance #stochastic

Here’s a clear, compact explanation of **Girsanov’s theorem** in English, with the key formulas you’ll actually use.

---

# Intuition (what it does)

Girsanov’s theorem tells you how to **change the probability measure** so that a process driven by Brownian motion gets a **different drift** (often removing it). Under the new measure, the “shifted” process is again a Brownian motion. This is the backbone of **risk-neutral pricing** in quantitative finance.

---

# Canonical setup (1-D)

Let $$(Ω,F,(Ft)t≥0,P)(\Omega,\mathcal F,(\mathcal F_t)_{t\ge0},\mathbb P)$$ carry a standard Brownian motion $$W_t.$$  
Let $$\theta_t$$ be an adapted process satisfying (e.g.) **Novikov’s condition**

$$EP ⁣[exp⁡ ⁣(12∫0Tθs2 ds)]<∞for all T>0.\mathbb E_{\mathbb P}\!\left[\exp\!\left(\tfrac12\int_0^T \theta_s^2\,ds\right)\right] < \infty \quad \text{for all } T>0.$$

Define the **stochastic exponential (Doléans–Dade exponential)**

$$Zt  =  exp⁡ ⁣(−∫0tθs dWs  −  12∫0tθs2 ds).Z_t \;=\; \exp\!\Big(-\int_0^t \theta_s\,dW_s \;-\; \tfrac12\int_0^t \theta_s^2\,ds\Big).$$

Then $$(Zt)t≥0(Z_t)_{t\ge0}$$ is a $$P\mathbb P-martingale$$ with $$Z0=1Z_0=1.$$ Use it to define a new measure $$Q\mathbb Q on FT\mathcal F_T$$ by the **Radon–Nikodym derivative**

$$dQdP∣FT  =  ZT.\frac{d\mathbb Q}{d\mathbb P}\Big|_{\mathcal F_T} \;=\; Z_T.$$

**Girsanov’s theorem:** Under $$\mathbb Q,$$

$$WtQ  :=  Wt+∫0tθs dsW_t^{\mathbb Q} \;:=\; W_t + \int_0^t \theta_s\,ds$$

is a standard Brownian motion. Equivalently, $$Wt=WtQ−∫0tθsdsW_t = W_t^{\mathbb Q} - \int_0^t \theta_s ds.$$

---

# Effect on SDEs

Consider the Itô SDE

$$dXt  =  b(t,Xt) dt  +  σ(t,Xt) dWt(under P).dX_t \;=\; b(t,X_t)\,dt \;+\; \sigma(t,X_t)\,dW_t \quad (\text{under } \mathbb P).$$

Under $$\mathbb Q$$ defined by $$\theta_t,$$ we can write $$dWt=dWtQ−θt dtdW_t = dW_t^{\mathbb Q} - \theta_t\,dt,$$ hence

$$dXt  =  [b(t,Xt)−σ(t,Xt) θt] dt  +  σ(t,Xt) dWtQ.dX_t \;=\; \big[b(t,X_t) - \sigma(t,X_t)\,\theta_t\big]\,dt \;+\; \sigma(t,X_t)\,dW_t^{\mathbb Q}.$$

So the **diffusion stays the same**, but the **drift changes by $$−\sigma\,\theta$$**.

> Vector form (multi-dimensional): if W is d-dimensional, $$θt∈Rd\theta_t\in\mathbb R^d and Zt=exp⁡ ⁣(−∫0tθs⊤dWs−12∫0t∥θs∥2ds)Z_t=\exp\!\big(-\int_0^t \theta_s^\top dW_s - \tfrac12\int_0^t \|\theta_s\|^2 ds\big).$$ Then $$WtQ=Wt+∫0tθsdsW^{\mathbb Q}_t = W_t + \int_0^t \theta_s ds$$ is a dd-dimensional Brownian motion under $$\mathbb Q,$$ and the drift shifts by $$-\sigma\theta $$(matrix–vector product).

---

# Finance application (risk-neutral measure)

Geometric Brownian motion under the “real-world” measure P\mathbb P:

$$dSt  =  μ St dt  +  σ St dWt.dS_t \;=\; \mu\,S_t\,dt \;+\; \sigma\,S_t\,dW_t.

Choose θ=μ−rσ\theta = \frac{\mu - r}{\sigma}.$$Under $$\mathbb Q$$ defined by this $$\theta,$$

$$dSt  =  r St dt  +  σ St dWtQ,dS_t \;=\; r\,S_t\,dt \;+\; \sigma\,S_t\,dW_t^{\mathbb Q},$$

so the **discounted price** $$e−rtSte^{-rt}S_t$$ is a **martingale**. Derivative prices are then

Price at $$t  =  EQ ⁣[e−r(T−t) payoffT ∣ Ft].\text{Price at } t \;=\; \mathbb E_{\mathbb Q}\!\big[e^{-r(T-t)}\,\text{payoff}_T \,\big|\, \mathcal F_t\big].$$

---

# Why the conditions matter

- **Novikov / Kazamaki conditions** ensure $$Z_t$$ is a true martingale (so $$\mathbb Q$$ is well-defined and equivalent to $$\mathbb P$$).
    
- **Equivalence of measures** preserves null sets and lets you transport almost sure properties.
    

---

# Minimal proof sketch

1. Define $$Z_t$$ as above and verify it’s a martingale (via Novikov).
    
2. Define $$\mathbb Q$$ by $$dQ=ZT dPd\mathbb Q = Z_T\,d\mathbb P.$$
    
3. Apply **Itô’s formula** to $$W_t$$ under $$\mathbb Q$$ using **Bayes/likelihood ratio** arguments (or Lévy’s characterization): show the candidate $$WtQ=Wt+∫0tθsdsW_t^{\mathbb Q}=W_t+\int_0^t\theta_s ds$$ has independent, stationary Gaussian increments and the right quadratic variation, hence is Brownian under $$\mathbb Q.$$
    

---

## TL;DR

Girsanov gives a recipe to **tilt the measure** so that a Brownian-driven SDE’s **drift changes by $$-\sigma\theta$$** while the driving noise remains Brownian under the new measure. This is exactly the move that yields the **risk-neutral measure** used for pricing.

---

O **teorema de Girsanov** é um dos resultados mais importantes da **teoria da probabilidade estocástica**, em particular para processos de Itô, e tem aplicações centrais em **finanças quantitativas** (mudança de medida de probabilidade para eliminar o risco em modelos de precificação de ativos).

---

### Intuição

O teorema de Girsanov diz que, se você tem um **movimento browniano** WtW_t sob uma medida de probabilidade P\mathbb{P}, então é possível **mudar a medida de probabilidade** para uma nova medida Q\mathbb{Q}, de forma que o processo WtW_t deixe de ter certo "drift" (tendência média) e passe a ser um movimento browniano puro sob Q\mathbb{Q}.

Em outras palavras:

- Sob P\mathbb{P}, o processo pode ter um drift.
    
- Sob Q\mathbb{Q}, esse drift pode ser "removido" (ou modificado).
    
- O preço que se paga por isso é mudar a medida de probabilidade, ajustando as expectativas com uma **densidade de Radon–Nikodym**.
    

---

### Formulação Matemática

Suponha que temos um processo estocástico dado por

dXt=μ dt+σ dWt,dX_t = \mu \, dt + \sigma \, dW_t,

onde WtW_t é um movimento browniano sob a medida P\mathbb{P}.

O teorema de Girsanov afirma que existe uma **nova medida de probabilidade** Q\mathbb{Q}, equivalente a P\mathbb{P}, tal que o processo

W~t=Wt+θt\widetilde{W}_t = W_t + \theta t

é um movimento browniano sob Q\mathbb{Q}, onde

θ=μσ.\theta = \frac{\mu}{\sigma}.

---

### Como funciona a mudança de medida

A densidade de Radon–Nikodym entre Q\mathbb{Q} e P\mathbb{P} é dada por

dQdP∣Ft=exp⁡ ⁣(−θWt−12θ2t).\frac{d\mathbb{Q}}{d\mathbb{P}} \Big|_{\mathcal{F}_t} = \exp\!\Big( -\theta W_t - \tfrac{1}{2} \theta^2 t \Big).

Isso é chamado de **exponencial estocástica**. Ela garante que as expectativas sob Q\mathbb{Q} sejam consistentes.

---

### Consequência prática

- Em **finanças**, isso é usado para passar da medida real P\mathbb{P} (que inclui o prêmio de risco) para a **medida neutra ao risco** Q\mathbb{Q}, na qual os preços descontados dos ativos seguem martingales.
    
- Isso permite que se faça **precificação de derivativos** simplesmente como expectativas sob Q\mathbb{Q}, sem precisar se preocupar com o drift real do ativo.
    

---

### Exemplo simples (modelo de Black-Scholes)

Sob P\mathbb{P}, o preço de uma ação segue

dSt=μSt dt+σSt dWt.dS_t = \mu S_t \, dt + \sigma S_t \, dW_t.

Pela mudança de medida de Girsanov, podemos passar para Q\mathbb{Q} em que

dSt=rSt dt+σSt dWtQ,dS_t = r S_t \, dt + \sigma S_t \, dW_t^{\mathbb{Q}},

onde rr é a taxa livre de risco.  
Sob Q\mathbb{Q}, o processo de preço descontado e−rtSte^{-rt}S_t é um martingale, o que é a base para precificação de opções.

---

🔑 **Resumo**:  
O teorema de Girsanov permite trocar de medida de probabilidade para transformar processos com drift em processos sem drift (ou com outro drift desejado). Isso é a espinha dorsal da teoria de precificação de ativos, porque permite construir a **medida neutra ao risco**.

---

Queres que eu faça também a **demonstração passo a passo** do teorema (com Radon–Nikodym, martingale exponencial etc.), ou preferes só a versão aplicada para finanças?