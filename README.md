# Monte-Carlo-Projects
Repo contenant le projet effectué dans le cadre du cours de Monte-Carlo for Finance du M2MO

Nous avons étudié l'article suivant : **"Strong convergence of the symmetrized
Milstein scheme for some CEV-like SDEs"** via l'introduction du schéma **"Symmetrized Milstein Scheme"** et nous avons étudié ses propriétés de convergence théorique (ordre de convergence) et les cadres d'utilisation de ces résultats. Nous avons également implémenté les différents résultats de l'article sur l'étude des ordres de convergence du schéma SMS et de quelques autres schémas classiques pour l'étude des diffusions CEV comme celui introduit par Aurélien Alfonsi (**Alfonsi Implicit Scheme**).

Lien vers le rapport : https://raw.githubusercontent.com/SamyMekk/Monte-Carlo-Projects/main/Projet%20Milstein/Projet_MC.pdf

<h1> <center >Etude des solutions des EDS de type CEV qui ont la forme suivante : </center> </h1>




<h3> <center> $dX_{t}=b(X_{t})dt+\sigma |X_{t}|^{\alpha}dW_{t}$   </center> </h3> 

- $\alpha \in [\frac{1}{2},1[$
- $x_{0} > 0$
- $\sigma > 0$

On va montrer que lorsque le rapport $\frac{b_{0}}{\sigma^{2}}$ est $\textbf{"assez élevé"}$ et que la fonction $b$ est assez régulière, on montre un ordre de convergence de 1 comme dans le cas classique des schémas de Milstein avec les coefficient Lipschitz et à croissance linéaire $C^{2}$ ce qui n'est pas le cas ici. Ainsi, l'hypothèse classique pour les ordres de convergence dans les schémas de Milstein ne s'appliquant pas ici, il n'est pas clair que l'on puisse retrouver un ordre de convergence de 1.


<h3> Introduction aux différents schémas de discrétisation  : </h3>

On introduit ici les différents schémas de discrétisation qui vont nous servir lors de l'implémentation avec une grille de temps donnée par $[0=t_{0},t_{1},...,t_{N}=T]$

<h5> Schéma SMS (Symetrized Milstein Scheme) : </h5>

- $\hat{X_{t_{0}}}=x_{0}$
- $\hat{X_{t_{k}}}=|\hat{X_{t_{k-1}}}+b(\hat{X_{t_{k-1}}})\Delta_{t}+\sigma \hat{X_{t_{k-1}}}(W_{t_{k}}-W_{t_{k-1}})+\alpha \frac{\sigma^{2}}{2}\hat{X_{t_{k}-1}}^{2 \alpha -1}[(W_{t_{k}}-W_{t_{k-1}})^{2}-\Delta t]|$ pour $k \in [1,N]$


<h5> Schéma SES (Symetrized Euler Scheme): </h5>


- $\hat{X_{t_{0}}}=x_{0}$
- $\hat{X_{t_{k}}}=|\hat{X_{t_{k-1}}}+b(\hat{X_{t_{k-1}}})\Delta_{t}+\sigma \hat{X_{t_{k-1}}}(W_{t_{k}}-W_{t_{k-1}})|$ pour $k \in [1,N]$


<h5> Schéma AIS : </h5>

Dans le cas $\alpha=0.5$, on introduit le schéma AIS dans le cadre d'un processus CIR comme suit :

$dX_{t}=(a-\kappa X_{t})dt+\sigma \sqrt{X_{t}}dW_{t}$

On définit alors le processus $X_{t}=Y_{t}^{2}$, bien défini par la condition de Feller supposée satisfaite

- $\hat{Y_{t_{0}}}=\sqrt{x_{0}}$
- $\hat{Y_{t_{k}}}=\frac{\hat{Y_{t_{k-1}}}+\frac{\sigma}{2}(W_{t_{k}}-W_{t_{k-1}})+\sqrt{(\hat{Y_{t_{k-1}}}+\frac{\sigma}{2}(W_{t_{k}}-W_{t_{k-1}}))^{2}+2(1+\frac{\kappa}{2}(t_{k}-t_{k-1}))(a-\frac{\sigma^{2}}{4})(t_{k} -t_{k-1})}}{2(1+\frac{\kappa}{2}(t_{k}-t_{k-1}))}$  pour $k \in [1,N]$
- $\hat{X_{t_{k}}}=\hat{Y_{t_{k}}}^{2}$ 



<h3> But du projet : </h3>

Dans le cadre du projet, on va comme dans l'article supposer que la fonction "drift" est linéaire donnée par : $b(x)=10-10x$. On va également considérer un horizon temporel $T=1$ et $x_{0}=1$

Nous allons nous intéresser à l'implémentation numérique des différents schémas numériques dans le cadre premièrement : $\alpha >0.5$ où nous étudierons les schémas $SMS$ et $SES$ et leurs ordres de convergence dans différentes configurations sur le choix du rapport $\frac{b_{0}}{\sigma^{2}}$ par rapport à  une fonction de $\alpha$ puis dans le cas $\alpha=0.5$ où nous étudierons les schémas $SMS$, $SES$ et $AIS$ où les conditions de convergence ne sont pas les mêmes que dans le cas $\alpha > 0.5$.

Pour le calcul des ordres de convergence, on va chercher à estimer la norme $L^{1}$ associée à chaque schéma constituant l'erreur forte. En notant $F_{T}=|X_{T}-\hat{X_{T}}|$ classiquement avec $\hat{X}$ issue du schéma de discrétisation et $X_{T}$ la vraie solution, on veut donc estimateur $E[F_{T}]$ pour chacun des schémas introduits.


- Comme il n'y a pas de solution explicite pour les différents schémas que l'on étudie, on va se donner dans le cas $\alpha=\frac{1}{2}$ comme solution de référence $X_{T}$ issue d'un schéma $AIS$ approximé avec un pas de temps de $\Delta t=\frac{\Delta_{max}}{2^{9}}$ (pour accélérer le temps d'éxécution)
- Dans le cas $\alpha > \frac{1}{2}$, on va se donner  comme solution de référence $X_{T}$ un schéma $SMS$ approximé avec un pas de temps de $\Delta t=\frac{\Delta_{max}}{2^{9}}$ ( pour accélérer le temps d'éxécution)

Ensuite, pour chaque $\Delta t \in [ {\frac{\Delta_{max}}{2^{n}}, n \in [1,9]}$], on va estimer les espérances par les moyennes empiriques pour chaque schéma en simulant **(5000 trajectoires)** par soucis de temps de calcul) et afin d'observer l'ordre de convergence, on va effectuer la régression linéaire de $log(F_{T})$ par rapport à $log(\Delta t)$ et le coefficient directeur représentera l'ordre de convergence et on représentera également graphiquement ces courbes. On introduira également la fonction **Identité** qui a un coefficient directeur de 1 qui servira de référence.














Incoming : **Malliavin Calculus**
