Relevant: [[typua Design]]

# Gradual Typing

# ?(Unknown) Type
型アノテーションを省略した場合、型システムはその式を?(Unknown)型と判定する。

- All Unknown
```lua
function func(x, y)
    return x + y
end
```

この例では`func`は2つの引数と返り値を持つ。  
型システムはそれぞれの引数と返り値を`Unknown`型と判定する。  


- Partial Unknown
```lua
---@param x number
function func(x, y)
    return x + y
end
```
In above case, Type System judge `x` is `number` type but `y` and retuyn value is `Unknown`


- All Annotated
```lua
---@param x number
---@param y number
---@return number
function func(x, y)
    return x + y
end
```

# 型の一貫性(Type Consistency)
## オブジェクト型の記法
オブジェクト型を表す記法を定義しよう。
オブジェクト型は以下のように定義する。

```math
\left[ l_1 : s_1, ..., l_n : s_n \right]
```

このとき $l_i$ はメソッド、 $s_i$ は型シグネチャとした

型シグネチャは $\tau \to \tau^{\prime}$ という記法で表示される。

本表記ではメンバ変数もメソッドとして取り扱うことに注意せよ。

例えば $x = 0$ というメンバ変数は $int \to int$ というメソッドとして解釈される。


## 制限演算子(Restriction Operator) $\sigma | \tau$
$\sigma | \tau$ は制限演算子(Restriction Operator)と呼ばれる。
この演算子は型 $\sigma$ のうち型 $\tau$ で不明な部分(すなわちUnknown型)をマスクする作用がある。
例えば

$$
\begin{align*}
    &\text{int} | _{?} = ? & \text{int} | _{\text{bool}} = int
\end{align*}
$$
$$
\left[ x: \text{int} \to \text{int}, y: \text{int} \to \text{int} \right] | _{ \left[ x: ? \to ?, y: \text{int} \to \text{int} \right] } = \left[ x: ? \to ?, y: \text{int} \to \text{int} \right]
$$

この演算子は再帰的に定義されている。

$$
\begin{align*}
  &\sigma | _\tau = \begin{cases}
	  (\_, ?) \Rightarrow ? \\
	  (\left[ l_1: s_1, ..., l_n:s_n \right], \left[ l_1: t_1, ..., l_n:t_n \right]) \Rightarrow \left[ l_1:s_1 | _{t_1} , ..., l_n:s_n | _{t_n} \right] \\
	  (\_, \_) \Rightarrow \sigma
  \end{cases}
\end{align*}
$$

$$
\left( \sigma_1 \to \sigma_2 \right) | _{\left( \tau_1 \to \tau_2 \right)} = \left( \sigma_1 | _{\tau_1} \right) \to \left(\sigma_2 | _{\tau_2} \right)
$$


## 型の無矛盾(Type Consistent)
2つの型 $\sigma$ と $\tau$ が
$$
\sigma | _{\tau} = \tau | _{\sigma}
$$
が成立するとき**無矛盾である**と言い、
$$
\sigma  \thicksim \tau
$$
と表記する。

# TypeCheck

# TypeInference

Incomplete type annotated case like following, TypeSystem may infer return value is `number`.

```lua
---@param x number
---@param y number
function func(x, y)
    return x + y
end

local a = func(1, 2) -- infer `a` is number
```

