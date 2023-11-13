YOUR NAME: 許綾恩  
YOUR ID: R12922115

## question 1 answer 
舉程式碼中的例子`c <== a*b;`相當於`c === a*b; c <-- a*b;`

`<--`的功能是assign`a*b`的結果給signal`c`
`===`的功能是建立quadratic constraint。確保算術電路中符合期望的結果

`c <== a * b;`讓`c`被assign為`a*b`的結果，並且也加入constraint來確保`c`確實是從`a*b`來的。

`<==`就是`===`(constraint generation)與`<--`(assignment)的結合，所以`<==`可以被用於assign值給signel並generate constraint確定計算在算術電路中會正確執行

## question 2 answer
`main.plonk.sol` 是circom編譯生成的智能合約。結合算術電路與區塊鏈，在區塊鏈上部署，可以驗證一些資訊像是：計算、交易合法性，在證明的過程中不用揭露所有細節，從而提升隱私性和安全性。

## question 3 answer
* zkey：zkey中有proving key和verification key。proving key用於生成證明，verification key用於確認這些證明的正確性。分成兩種keys確保安全性和隱私性。
* circuit：算術操作和邏輯判斷的組合，處理輸入並產生輸出。以程式碼為範例：circuit定義輸入與計算，再用hash作處理，無法反推的特性會抹去輸入細節。讓證明者可以透過特定的輸出向驗證者證明，不需要揭露輸入細節。

## question 4 answer
因為`e===f`，`f`會和`e`一樣，刪去`f`的內容再跑一次就可以得到輸出：
`17499677547561660273017699567908067415377678347145626859540034597523441084050`

## question 5 answer
1. `a`、`b` hash得到`hash.out`。
2. `c`、`d` hash得到`hash2.out`。
3. `hash.out`、`hash2.out` hash得到`e`(root)。

修改程式碼以印出所需值：
```circom!
pragma circom 2.1.6;

include "circomlib/poseidon.circom";

template MerkleTree () {
    signal input a, b, c, d;
    component hash = Poseidon(2);
    component hash2 = Poseidon(2);
    hash.inputs[0] <== a;
    hash.inputs[1] <== b;
    log(b);
    hash2.inputs[0] <== c;
    hash2.inputs[1] <== d;
    log(hash2.out);
}

component main = MerkleTree();
```
`a`的merkle proof path：
`[b,hash2.out]=
[77,16849369192409107332756158295736271172359908253560919913964223668672656406138]`
