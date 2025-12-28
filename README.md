## 推定量の構築
### 定義
- $r$: re75
- $f$: 予測モデル
- $X$: ユーザー特徴量
### (A)真の目的関数（解くべき問題）
```math
\begin{aligned}
\mathcal{J}(f_{\phi})
& = \mathbb{E}_i[l(r, f(X))|O_i=0]\\
& = \int l(r_i, f(X))\cdot P(X|O=0)dX\\
% ベイズの定理を用いると、
& = \int l(r_i, f(X))\cdot \frac{P(O=0|X)\cdot P(X)}{P(O=0)}dX~(ベイズの定理)\\
% エビデンス部分は定数なので無視し、O=1の確率を\theta(X)とおくと、
& \propto \int l(r_i, f(X))\cdot (1-\theta(X))\cdot P(X)dX~(定数であるエビデンスは無視)\\
& = \mathbb{E}[(1-\theta(X))\cdot l(r, f(X))]
\end{aligned}
```

### (B)ナイーブ推定量（観測データに等しく重みづけた二乗誤差）
```math
\begin{aligned}
\mathcal{J}_{naive}(f_{\phi})
& = \frac{1}{N}\sum_{i:O_i=1}^N l(r_i, f(X_i))\\
& = \frac{1}{N}\sum_{i:O_i=1}^N l(r_i, f(X_i))\cdot O_i\\
\mathbb{E}_i[\mathcal{J}_{naive}]
& = \frac{1}{N}\sum_{i:O_i=1}^N\mathbb{E}_i[l(r_i, f(X_i))\cdot O_i]\\
% theta(X)を用いると、
& = \mathbb{E}[\theta(X)\cdot l(r, f(X))]
% & = \mathbb{E}_i\left[\frac
\end{aligned}
```
**観測されやすいユーザーにおける誤差を大きく評価**しておりバイアスが生じる

### (C)IW推定量（期待値が真の目的関数と一致するよう重みづけを調整した二乗誤差)
```math
\begin{aligned}
\mathcal{J}_{IW}(f_{\phi})
& = \frac{1}{N}\sum_{i:O_i=1}^N \frac{1-\theta(X_i)}{\theta(X_i)}\cdot l(r_i, f(X_i))\\
\mathbb{E}_i[\mathcal{J}_{IW}]
& = \frac{1}{N}\sum_{i:O_i=1}^N\mathbb{E}_i\left[\frac{1-\theta(X_i)}{\theta(X_i)}\cdot l(r_i, f(X_i))\cdot O_i\right]\\
& = \mathbb{E}_i\left[(1-\theta(X))\cdot l(r, f(X))\right]\\
\end{aligned}
```
二乗誤差を$\frac{1-\theta(X)}{\theta(X)}$で重み付けることで、**期待値が(A)真の目的関数と一致**している
