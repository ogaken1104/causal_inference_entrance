## 推定量の構築
### 定義
- $m$: married (結婚有無)
- $f$: 予測モデル
- $X$: ユーザー特徴量
### 解くべき問題（真の目的関数）
$$
\begin{aligned}
\mathcal{J}(f_{\phi})
& = \mathbb{E}_i[l(m, f(X))|O_i=0]\\
& = \int l(m_i, f(X))\cdot P(X|O=0)dX\\
% ベイズの定理を用いると、
& = \int l(m_i, f(X))\cdot \frac{P(O=0|X)\cdot P(X)}{P(O=0)}dX\\
% エビデンス部分は定数なので無視し、O=1の確率を\theta(X)とおくと、
& \propto \int l(m_i, f(X))\cdot (1-\theta(X))\cdot P(X)dX\\
& = \mathbb{E}[(1-\theta(X))\cdot l(m, f(X))]
\end{aligned}
$$

### 観測データの平均から構築した推定量（ナイーブ推定量と呼ぶ）
$$
\begin{aligned}
\mathcal{J}_{naive}(f_{\phi})
& = \frac{1}{N}\sum_{i:O_i=1}^N l(m_i, f(X_i))\\
& = \frac{1}{N}\sum_{i:O_i=1}^N l(m_i, f(X_i))\cdot O_i\\
\mathbb{E}_i[\mathcal{J}_{naive}]
& = \frac{1}{N}\sum_{i:O_i=1}^N\mathbb{E}_i[l(m_i, f(X_i))\cdot O_i]\\
% theta(X)を用いると、
& = \mathbb{E}[\theta(X)\cdot l(m, f(X))]
% & = \mathbb{E}_i\left[\frac
\end{aligned}
$$
ナイーブ推定量の期待値は、観測されやすいユーザーにおける誤差を大きく評価しており、真の目的関数とバイアスがある。

### 期待値が真の目的関数と一致するよう重みづけを調整した推定量（IW推定量と呼ぶ）
$$
\begin{aligned}
\mathcal{J}_{IW}(f_{\phi})
& = \frac{1}{N}\sum_{i:O_i=1}^N \frac{1-\theta(X_i)}{\theta(X_i)}\cdot l(m_i, f(X_i))\\
\mathbb{E}_i[\mathcal{J}_{IW}]
& = \frac{1}{N}\sum_{i:O_i=1}^N\mathbb{E}_i\left[\frac{1-\theta(X_i)}{\theta(X_i)}\cdot l(m_i, f(X_i))\cdot O_i\right]\\
& = \mathbb{E}_i\left[(1-\theta(X))\cdot l(m, f(X))\right]\\
\end{aligned}
$$
