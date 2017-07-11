# リクルートライフスタイル Swiftコーディング規約

## このコーディング規約について
本コーディングガイドは、他社様の規約を参考にしつつ、リクルートライフスタイルのルールを盛り込んだものです。  
これから増えていくであろうSwift製のアプリケーションのソースコードについて、下記の達成を目標として策定しています。

* 出来る限り厳密で、プログラマ・レビュワーが誤解する可能性が少ないこと
* 冗長さが排除されていること
* 同一アプリ内はもちろんのこと、各アプリ毎のソースコードにおいても一貫性があること

なお、参考にさせて頂いた規約については項番13. に記載しています。  
参考にさせて頂いた規約と記述内容が似た箇所がありますが、本規約をご確認頂いて了承を頂いた上で公開しております。


## 1. Naming
### 1-1. 省略なしのキャメルケースで命名する
原則省略なしのキャメルケースで命名する。
Classe・Structure・Enumとcase・Protocol名は大文字で、
メソッド名・変数・定数は小文字で始める。

**Preferred:**
```swift
protocol RobotProtocol {
    func jump()
}

private let numberOfHands = 2
private let numberOfLegs = 2

class Robot {
    enum RobotHeightType {
        case Low, Middle, High
    }
    var jumpButton = UIButton()
    let weightKilograms: CGFloat = 1.2

}
```

**Not Preferred:**
```swift
private let NUM_OF_HANDS = 2
private let kNumOfLegs = 2

protocol robotprotocol {
    func Jump()
}

class robot {
    enum robotHeightType {
        case low, middle, high
    }
    var Jump_Button = UIButton()
    let WeightKilograms: CGFloat = 1.2
}
```

### 1-2. クラス名のプレフィックスは付けない
Swiftでは名前空間が存在するため、クラス名の衝突を避けるためにプレフィックスを用いる必要がない。  
クラス名のプレフィックスは付与しないこととする。

**Preferred:**
```swift
class SwiftClass {
    // Do something
}
```

**Not Preferred:**
```swift

class RLSSwiftClass {
    // Do something
}
```

**Not Preferred:**
```swift
class User {
    var lastName = ""
    var firstName = ""
    
    func changeName(lastName: String, firstName: String) {
        self.lastName = lastName
        self.firstName = firstName
    }
}
```

### 1-3. 第一引数は可読性を考慮して、必要であれば外部引数名を利用する

**Preferred:**
```swift
// 定義
func customMessageFromString(originalString: String) -> String
// 呼び出し
customMessageFromString(string)

// 定義
func changeColors(borderColor borderColor: UIColor, backgroundColor: UIColor)
// 呼び出し
changeColors(borderColor: UIColor.whiteColor(), backgroundColor: UIColor.redColor())
```

`customMessageFromString`メソッドは`String`を引数とすることがメソッド名から明らかであり、外部引数名を記載すると冗長になる。  
一方、`changeColors`メソッドは第一引数に外部引数名を指定しないとどの部分の色を指定しているのか明確でないため、
外部引数名を利用するべきである。

**Not Preferred:**
```swift
// 定義
func dateFromString(dateString dateString: String) -> NSDate
// 呼び出し
dateFromString(dateString: "2014-03-14")

// 定義
func changeColors(borderColor: UIColor, backgroundColor: UIColor)
// 呼び出し
changeColors(UIColor.whiteColor(), backgroundColor: UIColor.redColor())
```

### 1-4. 変数名にオプショナル関連の意味を持たせない
`optionalValue`・`unwrappedView`・`actualLabel`のように変数名にオプショナル関連の意味を持たせない。

**Preferred:**
```swift
var shopNameLabel: UILabel?
var shopName: String?
```

**Not Preferred:**
```swift
var optionalShopNameLabel: UILabel?
var wrappedShopName: String?
```

### 1-5. アメリカ英語表記を使う
Appleのリファレンスに合わせて、アメリカ英語表記を使うこと。

**Preferred:**
```swift
let flavor = "sweet"
```

**Not Preferred:**
```swift
let flavour = "sweet"
```

## 2. Types
### 2-1. `var`より`let`
そうできる限り常に`var`ではなく`let`を使う。

*理由*:
値が変更されないにも関わらず`var`を使うと、本当に値が変わらないかどうかのチェックが必要となり、非効率的かつ不要なリスクを孕むことになる。
`let`を指定することにより、値が変化し得ないことが明確になり、より安全なコードを書くことができる。

### 2-2. 左辺に可能な限り型を書かない
`CGFloat`や`Int16`など明示する必要がある場合以外はコンパイラの型推論を利用する。

**Preferred:**
```swift
let titleString = "Recruit Lifestyle"
let titleLabel = UILabel()
var groups = [String]()
let maxRatio: CGFloat = 106.5
```

**Not Preferred:**
```swift
let titleString: String = "Recruit Lyfestyle"
let titleLabel: UILabel = UILabel()
var groups: [String] = [String]()
let maxRatio = 106.5
```

### 2-3. シンタックスシュガーを使う
型指定はシンタックスシュガーを使って短い書き方を採用する。

**Preferred:**
```swift
var companyNames: [String]
var elements = [Int]()
var namesOfIntegers = [Int: String]()
```

**Not Preferred:**
```swift
var companyNames: Array<String>
var elements: [Int] = []
var elements: [Int] = [Int]()
var elements = Array<Int>()
var namesOfIntegers: [Int: String] = [Int: String]()
var namesOfIntegers = Dictionary<Int, String>()
```

### 2-4. Swiftネイティブな型を使う
そうできる限り常にSwiftネイティブな型を使う。

**Preferred:**
```swift
let percent: Double = 90.5                            // Double
let percentString = String(percent)                   // String
```

**Not Preferred:**
```swift
let percent: NSNumber = 90.5                          // NSNumber
let percentString: NSString = percent.stringValue     // NSString
```

### 2-5. キャストする場合は左辺の型を省略

**Preferred:**
```swift
let name = data["rls"] as String
```

**Not Preferred:**
```swift
let name: String = data["rls"] as String
```

### 2-6. Forced Unwrappingを避ける
`SomeType?`もしくは`SomeType!`型の変数`foo`がある場合、
`foo`に対してForced Unwrappingを行うのは可能な限り避ける。

**Preferred**
```swift
var userNameLabel = UILabel()
if let foo = foo {
    self.userNameLabel.text = foo
} else {
    // 必要であればnil時の処理
}
```
もしくは
```swift
guard let bar = foo else { return }
self.userNameLabel.text = bar
```

**Not Preferred**
```swift
var userNameLabel = UILabel()
self.userNameLabel.text = foo!
```

また、上記の`foo`にアクセスする場合、またメソッドを呼び出す場合は可能な限りOptional Chainingを使用する。

**Preferred**
```swift
foo?.doSomeTask()
```

**Not Preferred**
```swift
foo!.doSomeTask()
```


### 2-7. Implicitly Unwrapped Optional型（型の末尾に`!`が付く型）を避ける
もし`foo`が`nil`になる場合があるなら、可能な限り`let foo: SomeType?`を使う。
（大抵の場合、`!`の代わりに`?`を使えるはず。）
Implicitly Unwrapped Optional型は暗黙的にアンラップされるため、nilが入っていた場合
ランタイム時のクラッシュが発生してしまう。

## 3. Classes/Structures
### 3-1. `class`と`struct`の使い分け

下記のうち１つ、もしくはいずれかに当てはまる場合、`Class`ではなく`Struct`として定義する。

* 複数個の値型のデータをひとまとめにして扱う場合
* 継承が必要無い場合
* メソッドを持っているが、プロパティの簡単な操作を行うだけの処理である場合

**structの方が良いケースの例**
```swift
public struct DtoUser {
    var lastName = ""                 // property数が少なく、値型のみを持っている
    var firstName = ""
    
    public func displayName() -> String {
        return "\(self.lastName) \(self.firstName)"
    }
}
```

例えばValue ObjectやDTOは、基本的には単純に`Int`や `String`, `struct`等の値型を格納するだけ（そして場合によっては、propertyに対して文字列連結などの
簡単な処理を行う）のものである。
このような場合、意図しない値の変更を防ぐためにも`struct`として実装するべきである。

ただし、以下の場合については`class`にするか検討する必要がある。

* propertyに参照型がある場合
* 一意性の必要がある場合
* deinitializerが必要な場合
* 継承が必要な場合

**classの方が良いケースの例**
```swift
public class DtoUser {
    var lastName = ""
    var firstName = ""
    var userAttribute = ""
    var userGroup = UserGroupClass()  // Class(参照型)をpropertyとして持っている

    public func displayName() -> String {
        return "\(self.lastName) \(self.firstName)"
    }
}
```

### 3-2. トップレベルの定義にはアクセス制御を明示
```swift
public let globalNumber: Int
internal struct Company {}
private func doTasks(tasks: [Task]) {}
```

しかし、トップレベルの定義の中のものについてのアクセス制御は問題なければ暗黙的で良い。
```swift
internal struct Company {
    var establishedString = "2016/02/08"
}
```

*理由：*
トップレベルの定義が`internal`で適切な場合はあまりない。
アクセス制御に対して意識することにも繋がる。


### 3-3. クラス定義はfinalで
クラス定義はまず`final`指定して始める。 明確に継承の必要性が出た場合にのみ、`final`を外す。

理由：継承よりコンポジションの方が、好ましい場合が多い。継承の選択を決定する前にコンポジションで実現できないか再考したほうが良い。
また、動的ディスパッチを減らしてパフォーマンスを改善するという意味でも継承が必要でない場合はfinal指定をするのが望ましい。  

更に、プロトコル指向の考え方を用いて`struct`と`protocol`で実現できないかも考える余地がある。


### 3-4. 自身のプロパティ、メソッドにアクセスする際は`self`を付ける
理由：本規約ではプロパティ名とローカル変数名、メソッドの引数名に同一の名前を付けることを禁止していない（名前を変えるために冗長な名前を付けるのを避けるため、また、`guard let`や `if let`で同一の名前を付けることが一般的に行われるため）。  
そのため、selfを付けてコーダー自身が値を誤らないよう、また、レビュワーも誤らないように`self`を付けることとする。


```swift
class MyCompany {
    let name: String
    let genre: String
    
    init(name: String, genre: String) {
        self.name = name
        self.genre = genre
        let closure = {
            print(self.name)
        }
    }
}
```

### 3-5. プロトコル実装は別々のextensionで
プロトコルの実装は`class`部とは別に切り出し、`// MARK: -`も付ける。  
理由：関連メソッドをまとめることによる可読性の向上と、プロトコルの追加の容易さ向上のため。

**Preferred:**
```swift
class SampleViewController: UIViewController {
    // class contents
}

// MARK: - OtherViewControllerDelegate
extension SampleViewController: OtherViewControllerDelegate {
    // OtherViewControllerDelegate methods
}
```

**Not Preferred:**
```swift
class SampleViewController: UIViewController, OtherViewControllerDelegate {
    // All methods
}
```

### 3-6. `get`ブロックは必要な時のみ
可能な限り、読み取り専用のプロパティと添字付けではgetキーワードを省く。

**Preferred:**
```swift
var sampleNumber: Int {
    return 10
}
    
var squaredSampleNumber: Int {
    return self.sampleNumber * self.sampleNumber
}
```

**Not Preferred:**
```swift
var sampleNumber: Int {
    get {
        return 10
    }
}
    
var squaredSampleNumber: Int {
    get {
        return self.sampleNumber * self.sampleNumber
    }
}
```

### 3-7. グローバルな変数は宣言しない
定数についてはグローバルでも化。

**Not Preferred:**
```swift
var globalValue = "init value"

class Hoge {
  // use global value
}
```

## 4. Functions
### 4-1. 戻り値がVoidの場合は省略

**Preferred:**
```swift
func noReturnValueMethod(text: String) {
    // Do something
}
```

**Not Preferred:**
```swift
func noReturnValueMethod(text: String) -> () {
    // Do something
}

func noReturnValueMethod(text: String) -> Void {
    // Do something
}
```

### 4-2. 可能であれば早期リターンを使う

処理を続けるために特定の条件が必要な場合、guardによる早期リターンを行う。

**Preferred:**
```swift
guard text.isEmpty else { return }
// Use text
```

**Not Preferred:**
```swift
if text.isEmpty {
    // Use text
} else {
    return
}
```

nilの判定に`if`も使用することができるが、`guard`を使う。`guard`は`return`, `break`, `continue`, `throw`(メソッドに`throws`を付けている場合のみ)と共に使用しなければコンパイルエラーとなり、exitすることが保証されるため、こちらの方が好ましい。


## 5. Closures
### 5-1. 受け取るクロージャーが1つ、且つ最後にある場合のみtrailing closure構文を使う

**Preferred:**
```swift
UIView.animateWithDuration(5.0) {
    view.frame.size = CGSize(width: 100.0, height: 100.0)
}
        
UIView.animateWithDuration(5.0, animations: {
    view.frame.size = CGSize(width: 100.0, height: 100.0)
    }, completion: { bool in
         view.frame.size = CGSizeZero
})
```

**Not Preferred:**
```swift
UIView.animateWithDuration(5.0, animations: {
    view.frame.size = CGSize(width: 100.0, height: 100.0)
    }) { bool in
        view.frame.size = CGSizeZero
}
```

### 5-2. 処理が1行の場合はImplicit Returnを利用する

**Preferred:**
```swift
sampleArray.sort {
    $0 > $1
}
```

**Not Preferred:**
```swift
sampleArray.sort { (lhs, rhs) -> Bool in
    return lhs > rhs
}
```

## 6. Control Flow
### 6-1. `if`, `switch`, `while`文の条件は括弧で括らない

**Preferred:**
```swift
if a == b {
    // Do something
}
```

**Not Preferred:**
```swift
if (a == b) {
}
```

### 6-2. `for-in`構文を使う

**Preferred:**
```swift
for _ in 0..<3 {
    print("Hello three times")
}

for (index, member) in members.enumerate() {
    print("No. \(index) is \(member)")
}
```

**Not Preferred:**
```swift
for var i = 0; i < 10; i++ {
    print("\(i) times loop!!")
}

for var i = 0; i < members.count; i++ {
    let member = members[i]
    print("No. \(i) is \(member)")
}
```

*Note:*
Not Preferredに記載した、所謂C-styleのforループはSwift3.0で廃止される予定。
Swift2系以前のバージョンでコーディングをする際には、後でコンパイルエラーを起こさないためにも使用しないこと。

### 6-3 `++`, `--`演算子を使用しない
インクリメント、デクリメントについては `+=`, `-=`を用いる。

*Note:*
`++`, `--`の演算子はSwift3.0で廃止、もしくは前置と後置の差分が無くなる予定。
後でコンパイルエラーを起こさないためにも、また、前置後置の差分が紛らわしいので使用しないようにする。

## 7. Strings
### 7-1. 文字列長判定にはcharacters.countを使う
NSStringのlengthはUTF-16を2バイトコードとみなしてカウントしているため実際の文字数と異なることがある

**Preferred:**
```swift
"Swift text".characters.count
```

**Not Preferred:**
```swift
("Swift text" as NSString).length
```

### 7-2. ゼロレングスチェックはisEmptyを使う

**Preferred:**
```swift
if "Hoge".isEmpty {
  //
}
```

**Not Preferred:**
```swift
if "Swift text".characters.count == 0 {
    // Do something
}

if ("Swift text" as NSString).length == 0 {
    // Do something
}
```

## 8. Enum

### 8-1. rawValueが必要無い場合、`case`はカンマ区切りの1行で記述する
**Preferred:**
```swift
enum Result {
    case Success, Error
}
```

**Not Preferred:**
```swift
enum Result {
    case Success
    case Error
}
```

ただし、rawValueが必要な場合はcaseごとに改行する。

### 8-2. 型が自明な変数に代入する場合、enum名を省略する

**Preferred:**
```swift
myButton.addTarget(self, action: "someAction:", forControlEvents: .TouchUpInside)
var alignment: NSTextAlignment = .Center
```

**Not Preferred:**
```swift
myButton.addTarget(self, action: "someAction:", forControlEvents: UIControlEvents.TouchUpInside)
var alignment: NSTextAlignment = NSTextAlignment.Center
```

## 9. Comments
コードを読めば分かる内容のコメントは逆に分かりやすいコーディングを妨げるので不要。
必要な箇所に「なぜそのように書いたのか」を記述する。

### 9-1. Markdownで記述
以下にはMarkdownで役割・使い方・注意事項などを記述する。

* クラス定義
* プロトコル
* Enumerations
* Public Functions

**コメントサンプル**
```swift
/// ここにメソッドの説明を書く。
/// - parameter foo: 1つ目の引数。
/// - parameter bar: 2つ目の引数。
/// - returns: 処理結果が◯◯ならtrue, ××ならfalseを返す。
func doSomething(foo: String, bar:Int) -> Bool {
    // do something
    return false
}
```

## 10. Formatting
### 10-1. 改行コードはLF(XCodeのデフォルト)

### 10-2. インデントは4スペース

### 10-3. ファイル終端は改行

### 10-4. 行末に空白を残さない

### 10-5. セミコロンは付けない
1行1文で、文末尾にはセミコロンを付けない。

**Preferred:**
```swift
let text = "Do not write semicolon at the end of the line"
```

**Not Preferred:**
```swift
let text = "Do not write semicolon at the end of the line";
```

### 10-6. コードブロックの順番
1. enumerations
2. Properties
3. IBOutlets
4. IBActions
5. Initializers
6. LifeCycle
7. Public Methods
8. Internal Methods
9. Private Methods
10. Segues

### 10-7. ブレース(`{`, `}`)は前にスペースを1つ空けて行末に記述する

**Preferred:**
```swift
if myInstance.isEmpty {
  // Do something
} else {
  // Do something else
}
```

**Not Preferred:**
```swift
if myInstance.isEmpty
{
  // Do something
}
else {
  // Do something else
}
```

### 10-8. スペースの挿入ルール
#### 10-8-1. ブレース(`{`, `}`)の前後には半角スペースを挿入する

#### 10-8-2. 型指定や`Dictionary`の定義、値の挿入の際、コロンの前にはスペースを入れず、コロンの後にスペースを挿入する

**Preferred:**
```swift
let radius: CGFloat = 5.0
var shapes = [Int: String]()
var checkList = [
    1: true,
    2: false
]
```

**Not Preferred:**
```swift
let radius:CGFloat = 5.0
var shapes = [Int :String]()
var checkList = [
    1 : true,
    2 :false
]
```
#### 10-8-3. カンマの前にはスペースを入れず、カンマの後にスペースを挿入する

#### 10-8-4. 二項演算子の前後には半角スペースを挿入する

### 10-9. 1行180文字以内を目安とし、それを超える場合は可読性を考慮して適切な位置に改行を入れる

## 11. Cocoa
### 11-1. NSLocalizedStringのキー名は、状況に応じてわかりやすい、ユニークなキー名を付ける

```swift
NSLocalizedString("Error.Auth.Title", comment:"Authentication error")
NSLocalizedString("AmountDetail.Title.Starting", comment: "starting")

NSLocalizedString("DailyVoucher.Alert.Title", comment:"Title")
NSLocalizedString("DailyVoucher.Alert.Cancel", comment:"Cancel")
```

## 12. Contributors
* [Yuki Nagai](https://github.com/uny)
* [Hiroaki Asano](https://github.com/mokemoko)
* [Koji Furuya](https://github.com/furuyan)
* [Daisuke Nakagawa](https://github.com/daisukeNakagawa)
* [Takayuki Hara](https://github.com/takayuki-hara)

## 13. References
1. [raywenderlich](https://github.com/raywenderlich/swift-style-guide)
1. [github](https://github.com/github/swift-style-guide)
3. [SlideShareInc](https://github.com/SlideShareInc/swift-style-guide)
4. [Wantedly](http://qiita.com/susieyy/items/f71435cc962e70d81b37)
5. [String Localization - Strings - objc.io issue](http://www.objc.io/issues/9-strings/string-localization/)
