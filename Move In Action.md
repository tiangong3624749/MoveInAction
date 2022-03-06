## 加密货币 & ERC20实战

加密货币（Cryptocurrencies）是一种使用密码学原理来确保交易安全及控制交易单位创造的交易媒介。BTC是世界上第一个加密货币，是给那些维护Bitcoin网络安全的人（矿工，miner）的一种激励。现在通常把使用区块链技术发行的数字货币统称为加密货币。

那么，目前区块链行业，如何通过技术手段发行加密货币呢？

在区块链技术高速发展的十多年中，有两个最核心的底层技术，分别是去中心化（Decentralized）和智能合约（Smart Contract）。围绕这两项核心技术，产生了丰富的应用和繁荣的生态。尤其最近两年异常火爆的DeFi（英文全称是Decentralized Finance，中文名称叫去中心化金融），更是离不开去中心化和智能合约等区块链技术。

去中心化本质上是在一个开放的分布式系统中，如何保障数据的安全性和最终一致性。在广域网中，任何时间任何地点都可能存在一些计算机（通常称之为节点），要在尽量短的网络延迟内，保证这些节点之间数据的最终一致性，这是一个世界性难题。区块链引入了一种激励措施，让分散在世界各地的大量节点主动来维护网络的安全（数据安全和最终一致），这是加密货币的一种发行方式，也是目前公链常用的手段。

智能合约是另一种常用的发行加密货币的方式。智能合约概念于1995年由尼克·萨博（Nick Szabo）首次提出，一个智能合约是一套以数字形式定义的承诺，合约参与方可以在上面执行这些承诺的协议。事实上，智能合约比区块链早诞生十多年。受限于Bitcoin Script的表达能力，2014年维塔利克·布特林（Vitalik Buterin）在以太坊（Ethereum）中引入智能合约，智能合约才开始大放异彩进而广为人知。

在区块链的技术里，谈到智能合约通常是指一门图灵完备的编程语言。目前最受欢迎的是以EVM（Ethereum Virtual Machine）作为运行环境的智能合约语言Solidity，本文后面要展开介绍的Move是目前最具创新的一门智能合约语言。接下来，将介绍通过智能合约发行加密货币。



### 1. Ethereum & 智能合约

Bitcoin首创了加密货币，并且给加密货币赋予了去中心化的特性。Ethereum则首度引入智能合约，让加密货币拥有了更好的表达能力。这样，加密货币不仅仅是一种奖励，更是一种能够自由表达的数字货币。

在Ethereum上，目前除了最常用的Solidity之外，还有Vyper等其他智能合约语言。Solidity是一种高级编程语言，可以用于编写合约代码（关于Solidity的语法规则不是本文重点，感兴趣的可以查看Solidity语法书籍和资料）。然后再使用solc编译器，将编写好的Solidity合约代码转换为相应的字节码，最后将编译后的字节码部署到Ethereum的合约账户的Code区。当用户发起交易调用合约时，Ethereum提供了EVM虚拟机来运行合约对应的字节码，达到更新链上用户状态的目的（特别说明的是，EVM是完全隔离的，也就是说在EVM中运行的合约代码无法访问网络、文件系统和其他的进程）。

所以，任何人如果有需要，都可以根据自己的需求选择一门合适的智能合约语言（例如Solidity）来编写属于自己的智能合约代码，并部署到Ethereum上，例如发行代币。

智能合约让区块链拥有了自由表达的能力。在Ethereum上，任何人都可以使用Solidity等基于EVM的智能合约语言来自由的表达自己想要的需求。为了促进Ethereum生态的良性发展，Ethereum社区使用ERC（全称是Ethereum Request for Comment）意见征求稿的形式，讨论并制定出了一系列行业标准并放到EIP（Ethereum Improvement Proposals）清单里。

在所有针对合约层的Ethereum标准中，最著名的标准莫过于ERC20和ERC721这两个了。ERC20和ERC721是两个最基本的Token标准，其中，ERC20用于定义同质化代币（Fungible Token，以下简称FT），而ERC721用于定义非同质化代币（Non-Fungible Token，也就是常说的NFT，NFT不是本章的重点）。除了NFT和公链原生的加密货币之外，目前市场上大部分加密货币都会遵循ERC20的标准，可以说，ERC20是FT一个事实上的行业标准了。接下来，将会更重点讲述一下发行FT的ERC20标准。



### 2. ERC20协议

最近几年，在智能合约彻底释放了区块链的表达能力之后，DeFi赛道空前火爆，各种各样的应用层出不穷，例如流动性挖矿、算法稳定币等等。在这些流行的应用背后，通常都能够看到遵循ERC20标准的FT。

在Ethereum生态甚至是EVM生态中，ERC20都是一种通用的FT标准协议，定义了FT需要遵守的规范。以下是一个ERC20标准，这里会进行简单的介绍，想要更深入了解的读者可以去查看Ethereum的[EIP-20](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md)。

~~~solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

/**
 * https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md
 */
interface IERC20 {

		function name() public view returns (string);
		
		function symbol() public view returns (string);
		
		function decimals() public view returns (uint8);
		
    function balanceOf(address _owner) external view returns (uint256 balance);

    function transfer(address _to, uint256 _value) external returns (bool success);

    function transferFrom(address from, address to, uint256 value) external returns (bool success);

    function approve(address spender, uint256 value) external returns (bool success);

    function allowance(address owner, address spender) external view returns (uint256 remaining);

    event Transfer(
        address indexed from,
        address indexed to,
        uint256 value
    );

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}
~~~

上面的ERC20的interface中，主要包含3部分重要的信息：

1、FT的基本信息

name表示当前FT的名称，例如MyToken；symbol表示当前FT的符号，例如HIX；decimals表示当前FT的精度，例如8。

2、操作规范

balanceOf，用于查看用户的余额；

transfer，用户转账，注意判断返回值；

transferFrom，更安全的用户转账函数，注意判断返回值；

3、FT状态变更的Event

Transfer，转账的Event；Approval，批准的Event；



### 3. ERC20实例

前面简单了解了ERC20协议，我们通再过一个简单的发行FT的实例来进一步理解一下ERC20。

下面的例子中，发行了一个MyToken的FT。MyToken的符号是MYT，精度是6。

~~~solidity
pragma solidity >=0.7.0 <0.9.0;

contract MyToken is IERC20 {

    uint256 constant private MAX_UINT256 = 2**256 - 1;
    mapping (address => uint256) public balances;
    mapping (address => mapping (address => uint256)) public allowed;

    uint256 public totalSupply;
    string public name = 'MyToken';      //  token name
    string public symbol = 'MYT';           //  token symbol
    uint256 public decimals = 6;            //  token digit

    constructor(
        uint256 _totalSupply
    ) {
        balances[msg.sender] = _totalSupply;
        totalSupply = _totalSupply;
    }

    function transfer(address _to, uint256 _value) public override returns (bool success) {
        require(balances[msg.sender] >= _value);
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public override returns (bool success) {
        uint256 remaining = allowance(_from, msg.sender);
        require(balances[_from] >= _value && remaining >= _value);
        balances[_from] -= _value;
        balances[_to] += _value;
        if (remaining < MAX_UINT256) {
            allowed[_from][msg.sender] -= _value;
        }
        emit Transfer(_from, _to, _value);
        return true;
    }

    function balanceOf(address _owner) public override view returns (uint256 balance) {
        return balances[_owner];
    }

    function approve(address _spender, uint256 _value) public override returns (bool success) {
        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function allowance(address _owner, address _spender) public override view returns (uint256 remaining) {
        return allowed[_owner][_spender];
    }
}
~~~

通过is关键字，能够看到上面是对interface ERC20的一个简单实现。这里不对Solidity的语法进行介绍，感兴趣的可以查看Solidity的相关文档。

我们重点关注一下transfer函数的实现。前面的transfer函数实现了从msg.sender账户转账给_to账户的功能，是没有问题的。下面提供了transfer函数的另一种实现：
~~~solidity
    function transfer(address _to, uint256 _value) public override returns (bool success) {
        require(balances[msg.sender] >= _value);
        uint256 oldFromVal = balances[msg.sender];
        uint256 oldToVal = balances[_to];
        uint256 newToVal = oldToVal + _value;
        uint256 newFromVal = oldFromVal - _value;
        balances[msg.sender] = newFromVal;
        balances[_to] = newToVal;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }
~~~

从功能上来讲，两种实现是完全一致的。但是第二种实现其实是有问题的，存在重大的安全漏洞。当msg.sender账户和_to账户是同一个账户的时候，newToVal的值会覆盖newFromVal的值，相当于前面的扣款逻辑无效，并且凭空给这个用户增加了数量为value的MyToken。实际上，这是一个真实存在的无限增发的安全漏洞，还有很多其他真实的安全漏洞，感兴趣的可以去了解一下。



### 4. ERC20的总结

可以说，ERC20协议是Solidity生态被广泛接受的一个底层标准，很多应用遵循ERC20协议来发行FT。正是因为FT如此的重要，所以安全愈加珍贵。

如果从安全的的角度分析，ERC20协议其实并不完美（从上面的无限增发漏洞能够体会到），存在一些使用上需要特别注意的问题：

1、将FT转账给合约地址将导致FT进入黑洞；

2、发行的FT被集中存储在智能合约中，出现安全漏洞会影响所有用户的FT的安全；

3、合约发行的FT不能跨合约，不能作为其他合约的数据；

4、FT只是一个数字，没有任何安全保护，在传输过程中容易出现安全隐患，比如溢出或者被篡改或者被拷贝；

5、需要根据函数返回结果，来判断转账等操作是否成功；

除了上面提到的这些问题，还有一些其他的潜在问题。这些问题变相提高了对开发者的要求，在后面将会介绍更简单、更安全的FT协议。



## Move & Token

公链的原生Token安全性是由共识来保障的，例如ETH，而遵循ERC20协议的Token，安全性则完全依赖智能合约。所以，智能合约语言的安全性就显得尤为重要。Solidity作为最早出现的智能合约语言之一，有很好的灵活性，但是在安全方面有很多的隐患：

1、函数默认的可见性是public，容易因为遗忘，导致权限泄露，例如Parity的钱包漏洞；

2、在调用其他合约的时候，需要注意msg上下文的切换，避免留下安全隐患；

3、Fallback函数也存在一定的隐患，例如著名的TheDAO攻击，通过Fallback函数构造了递归调用；

4、数字溢出不报错（在Solidity后面的版本中已经修复了）；

5、数据集中存储在同一个合约中，容易产生大数组等安全问题；

实际上，Solidity还有很多其他潜在的安全隐患需要开发者格外注意。从根本上来说，智能合约场景，尤其是DeFi场景，Token是一个核心需求。相比传统金融场景，开源且去中心化的智能合约所面临的挑战要高得多，所以最好是有一门专门根据金融场景设计的、安全性更高的智能合约语言。

截止目前，Move是唯一一个专门为金融场景打造的智能合约语言，有丰富的安全特点：
1、更安全的资源类型，资源类型受到Move虚拟机的保护；

2、形式化验证，用数学手段证明合约的安全性；

3、除了基本数据类型，Struct结构体也能够跨合约使用；

4、面向泛型编程，让程序可复用、可扩展；

5、纯静态类型，无法构建递归，让程度更清晰和安全；

6、Struct结构体受保护，只能当前合约修改，其他合约没有修改的能力；

7、鼓励分散存储用户数据，让数据所有权更明确，践行Web3的理念；

除了上面这些特点，Move还有很多其他特点，例如溢出报错、更丰富的函数可见性等等。我们能够体会到，Move充分吸取了Solidity的经验和教训，并且结合金融场景的特点，将安全的理念发挥得淋漓尽致。接下来，我们将深入到Move，并且通过FT实例的实践，来近距离体验一下Move以及安全的FT。



### 1. Move

在动手之前，我们需要先搭建Move的开发环境。需要特别说明的是，目前暂时只有Starcoin（后面会简单介绍）和Diem（Libra改名成Diem）两个公链支持Move，其中Starcoin支持任何人部署Move合约，而Diem不开放Move合约部署功能，所以后面的例子将围绕Starcoin和Move展开。

Move介绍

环境搭建+IDE插件



### 2. MyToken实现

有了Move的开发环境，我们将从一个简单的发行FT的实例入手。我们先来想象一下将要发行的FT是什么样的？

首先，发行的FT具备基本的Token功能，例如转账、取款、存款等等；其次，尽可能的保证FT的安全性，不会出现那些低级的安全漏洞，例如无限增发、溢出漏洞、进入黑洞等等；第三，发行的FT能够被其他合约使用；

上面是我们对FT的一个基本需求（比ERC20的要求更高）。假设这个发行的FT叫MyToken，下面是它的一个实现：

~~~Move
address 0x2 {
module MyToken {
    use 0x1::Signer;

    struct MyToken has key,store {
        value: u128,
    }

    struct TokenInfo has key {
        total_value: u128,
    }

    const MINT_ADDRESS: address = @0x2;

    const ETOKEN_CAP_ER:u64 = 100;
    const INIT_ER:u64 = 101;
    const NOT_ACCEPT_ER:u64 = 102;
    const EAMOUNT_EXCEEDS_COIN_VALUE: u64 = 103;

    public(script) fun init(account: signer) {
        let myself = Signer::address_of(&account);
        assert(myself == MINT_ADDRESS, ETOKEN_CAP_ER);
        assert(!exists<TokenInfo>(myself), INIT_ER);
        move_to(&account, TokenInfo {total_value: 0},);
    }

    public fun admin() : address {
        MINT_ADDRESS
    }

    public fun mint(account: &signer, amount: u128): MyToken acquires TokenInfo {
        assert(Signer::address_of(account) == MINT_ADDRESS, ETOKEN_CAP_ER);
        let info = borrow_global_mut<TokenInfo>(MINT_ADDRESS);
        info.total_value = info.total_value + amount;
        MyToken { value:amount }
    }

    public fun zero(): MyToken {
        MyToken { value: 0 }
    }

    public fun value(token: &MyToken): u128 {
        token.value
    }

    public fun transfer(receiver: address, token: MyToken) acquires MyToken {
        assert(exists<MyToken>(receiver), NOT_ACCEPT_ER);
        let my_token = borrow_global_mut<MyToken>(receiver);
        Self::join(my_token, token);
    }

    public fun accept(account: signer) {
        move_to(&account,Self::zero(),);
    }

    public fun deposit(account: signer, token: MyToken) acquires MyToken {
        let myself = Signer::address_of(&account);
        if(!exists<MyToken>(myself)) {
            Self::accept(account);
        };
        Self::transfer(myself, token);
    }

    public fun withdraw(account: signer, amount: u128,): MyToken acquires MyToken {
        let myself = Signer::address_of(&account);
        assert(exists<MyToken>(myself), NOT_ACCEPT_ER);
        let token = borrow_global_mut<MyToken>(myself);
        Self::split(token, amount)
    }

    public fun join(token1: &mut MyToken, token2: MyToken) {
        let MyToken {value:value} = token2;
        token1.value = token1.value + value;
    }

    public fun split(token: &mut MyToken, amount:u128) :MyToken {
        assert(token.value >= amount, EAMOUNT_EXCEEDS_COIN_VALUE);
        token.value = token.value - amount;
        MyToken { value: amount }
    }
}
}
~~~

我们简单介绍一下上面的核心代码。

上面假设是0x2账户（这里的Admin账户，后面的合约也将用这个账户，用户根据自己的实际情况而定），FT全称叫0x2::MyToken::MyToken。TokenInfo维护了FT的总发行量total_value，只有一份，保存在当前账户下。只有当前账户可以调用mint函数发行MyToken。其他账户可以随时查看，不能修改。

上面定义的MyToken只有key和store的ability，这意味着MyToken没有copy和drop，所以MyToken不能拷贝和丢弃，将不会出现无限增发或者进入黑洞等异常情况。任何账户都可以在自己账户下持有上面定义的MyToken，并且对MyToken进行transfer转账、deposit存款、withdraw取款等操作，还可以通过split找零、join合并。

以上是MyToken合约定义的主要逻辑，代码简洁、功能丰富、Token安全：

* MyToken没有copy的ability，不能通过拷贝来进行无限增发
* MyToken没有drop的ability，不能凭空消失
* Move溢出会报错，不需要开发者格外处理
* MyToken可以分散存储到用户自己的账户下，避免漏洞导致的集中损失
* MyToken只能通过上面定义的合约进行修改，例如转账、取款、存款等等



### 3. MyToken & 专款专用

有了上面的定义的安全的MyToken，我们设想一个基于这个FT的应用场景，真实的感受一下结构体跨合约的能力以及MyToken的安全性。

假设成立了一个教育基金，使用票据和MyToken进行结算。内部单位使用票据进行结算，例如上级部门拨款给下级部门。跟市场结算，则使用MyToken，例如用MyToken购买书本。

这是我们的核心业务需求，背后有一个隐藏的需求是安全性。下面使用Move来实现这些需求：

~~~Move
address 0x2 {
module EducationFuund {
    use 0x1::Signer;
    use 0x2::MyToken::{Self, MyToken};

    struct Edu has key,store {
        token: MyToken,
    }

    const EDU_ACCEPT_ER:u64 = 200;
    const NOT_ADMIN_ER:u64 = 201;

    public fun accept(account: signer) {
        let myself = Signer::address_of(&account);
        assert(!exists<Edu>(myself), EDU_ACCEPT_ER);
        move_to(&account, Edu {token: MyToken::zero()},);
    }

    public fun mint(account:signer, token:MyToken) acquires Edu {
        let myself = Signer::address_of(&account);
        assert(myself == MyToken::admin(), NOT_ADMIN_ER);
        if(!exists<Edu>(myself)) {
            Self::accept(account);
        };
        let edu = borrow_global_mut<Edu>(myself);
        MyToken::join(&mut edu.token, token);
    }

    public fun appropriation(account:signer, receiver: address, amount:u128) acquires Edu {
        assert(exists<Edu>(receiver), EDU_ACCEPT_ER);
        let myself = Signer::address_of(&account);
        let my_edu = borrow_global_mut<Edu>(myself);
        let token = MyToken::split(&mut my_edu.token, amount);
        let receiver_edu = borrow_global_mut<Edu>(receiver);
        MyToken::join(&mut receiver_edu.token, token);
    }

    public fun buy_book(account: signer, amount:u128) : MyToken acquires Edu {
        let myself = Signer::address_of(&account);
        assert(exists<Edu>(myself), EDU_ACCEPT_ER);
        let edu = borrow_global_mut<Edu>(myself);
        MyToken::split(&mut edu.token, amount)
    }

    public fun balance(account:address) :u128 acquires Edu {
        let edu = borrow_global<Edu>(account);
        MyToken::value(&edu.token)
    }
}
}
~~~

上面在0x2的账户下，定义了一个EducationFuund模块，实现了教育基金的全部需求。

Edu是EducationFuund模块中唯一定义的结构体，表示一种票据。Edu只有key和store的ability，同样意味着Edu没有copy和drop，不能拷贝和丢弃，不会出现无限增发或者进入黑洞等异常情况。

当前账户通过mint注入资金。任何用户可以通过appropriation函数将票据转给其他账户，例如上级部门给下级部门拨款。任何账户可以拿着票据去市场交易，例如buy_book买书。

上面的合约非常简洁和安全，我们看到：

* 发行的MyToken被另一个合约EducationFuund使用
* EducationFuund并没有直接修改MyToken的权利，只能调用MyToken模块提供的函数进行修改
* Edu票据非常的安全可靠



### 4. MyToken总结

前面我们使用智能合约语言Move定义了一个“虽然代码简单，但是安全可靠”的FT实例MyToken，并且通过一个教育基金EducationFuund的例子，介绍了跨合约使用MyToken，给MyToken赋予了真实的价值（如果你想，可以构造更多、更大的场景）。这些是Solidity难以做到的。

可以说，智能合约语言Move不只是一门安全的智能合约语言，而且能够通过模块相互依赖的方式，构建复杂系统的应用。



## Starcoin & Move

继Solidity解放智能合约的表达能力之后，Move又把智能合约语言的能力提升了一个大的台阶。

Starcoin是首个使用Move作为智能合约语言的无许可公链。Starcoin使用Move实现了一个Starcoin Framework，包含了一些基础的协议，例如Token协议、NFT协议等等。任何人可以基于Starcoin以及Starcoin Framework开发和部署属于自己的Move合约。关于Starcoin的更多介绍，请查看其它资料。接下来，我们从Move和FT的角度，了解一下Starcoin的Token协议。



### 1. Starcoin的Token协议

Starcoin发布了官方的Token标准，并且提供了Move的实现。

我们来简单介绍一下Starcoin的Token协议：

~~~Move
		//////////// Token Info ////////////
    
    struct TokenCode has copy, drop, store {
        /// address who define the module contains the Token Type.
        addr: address,
        /// module which contains the Token Type.
        module_name: vector<u8>,
        /// name of the token. may nested if the token is a instantiated generic token type.
        name: vector<u8>,
    }
    
    struct TokenInfo<phantom TokenType> has key {
        /// The total value for the token represented by
        /// `TokenType`. Mutable.
        total_value: u128,
        /// The scaling factor for the coin (i.e. the amount to divide by
        /// to get to the human-readable representation for this currency).
        /// e.g. 10^6 for `Coin1`
        scaling_factor: u128,
        /// event stream for minting
        mint_events: Event::EventHandle<MintEvent>,
        /// event stream for burning
        burn_events: Event::EventHandle<BurnEvent>,
    }
    
    public fun scaling_factor<TokenType: store>(): u128 acquires TokenInfo 
    
    public fun market_cap<TokenType: store>(): u128 acquires TokenInfo
    
    public fun is_registered_in<TokenType: store>(token_address: address): bool
    
    public fun is_same_token<TokenType1: store, TokenType2: store>(): bool
    
    public fun token_address<TokenType: store>(): address
    
    public fun token_code<TokenType: store>(): TokenCode
		
		//////////// Token ////////////
		
		struct Token<phantom TokenType> has store {
        value: u128,
    }
		
		public fun register_token<TokenType: store>(account: &signer,precision: u8,)
		
		public fun mint<TokenType: store>(account: &signer, amount: u128): Token<TokenType> acquires TokenInfo, MintCapability
    
    public fun mint_with_capability<TokenType: store>(_capability: &MintCapability<TokenType>,amount: u128,): Token<TokenType> acquires TokenInfo
		
		public fun burn<TokenType: store>(account: &signer, tokens: Token<TokenType>) acquires TokenInfo, BurnCapability
    
    public fun burn_with_capability<TokenType: store>(_capability: &BurnCapability<TokenType>,tokens: Token<TokenType>,) acquires TokenInfo
    
    public fun zero<TokenType: store>(): Token<TokenType>
    
    public fun value<TokenType: store>(token: &Token<TokenType>): u128
    
    public fun split<TokenType: store>(token: Token<TokenType>,value: u128,): (Token<TokenType>, Token<TokenType>)
    
    public fun withdraw<TokenType: store>(token: &mut Token<TokenType>,value: u128,): Token<TokenType>
    
    public fun join<TokenType: store>(token1: Token<TokenType>,token2: Token<TokenType>,): Token<TokenType>
    
    public fun deposit<TokenType: store>(token: &mut Token<TokenType>, check: Token<TokenType>)
    
    public fun destroy_zero<TokenType: store>(token: Token<TokenType>) 
    
    //////////// Token Capability ////////////
    
    struct MintCapability<phantom TokenType> has key, store { }
    
    struct BurnCapability<phantom TokenType> has key, store { }
    
		public fun remove_mint_capability<TokenType: store>(signer: &signer): MintCapability<TokenType> acquires MintCapability 
    
    public fun add_mint_capability<TokenType: store>(signer: &signer, cap: MintCapability<TokenType>) 
    
    public fun destroy_mint_capability<TokenType: store>(cap: MintCapability<TokenType>)
    
    public fun remove_burn_capability<TokenType: store>(signer: &signer): BurnCapability<TokenType> acquires BurnCapability
    
    public fun add_burn_capability<TokenType: store>(signer: &signer, cap: BurnCapability<TokenType>)
    
    public fun destroy_burn_capability<TokenType: store>(cap: BurnCapability<TokenType>)
   	
   	//////////// Event ////////////
   	
   	struct MintEvent has drop, store {
        /// funds added to the system
        amount: u128,
        /// full info of Token.
        token_code: TokenCode,
    }

    struct BurnEvent has drop, store {
        /// funds removed from the system
        amount: u128,
        /// full info of Token
        token_code: TokenCode,
    }
~~~

上面的Token协议包括4个部分：

1、Token的基本信息，例如TokenInfo、TokenCode；

2、Token以及Token的基本操作，例如注册、取款、存款、燃烧等等；

3、Token的权限，例如MintCapability和BurnCapability；

4、事件，包括MintEvent和BurnEvent；

Starcoin不仅仅定义了Token协议，并且使用Move实现了Token协议，具体实现请参考[这里](https://github.com/starcoinorg/starcoin-framework/blob/main/sources/Token.move)。Starcoin的Token协议是一个通用的协议，有很多的特点：

* 泛型编程，不依赖于具体的实现
* Token只有store的ability，既不能copy，也不能drop，非常的安全可靠
* 丰富的功能，例如注册FT
* 精细化的权限管理
* 简介易用，任何人可以复用代码

可以看出，Starcoin的通用Token协议比前面自定义的MyToken合约要强大很多，比Ethereum的ERC20协议也要强大很多。



### 2. 基于Token协议的MyToken例子

前面介绍了一个安全强大的Token协议，这里，我们了解一下，如何基于Starcoin的Token协议快速实现一个简单的FT？

为了便于区分，假设这里是MyToken2。StarcoinFramework已经帮我们实现了Token的基本功能，所以，我们只需要根据我们的需求，定义好MyToken2，然后调用Token协议来注册和Mint铸造MyToken2，就可以快速实现我们FT。下面是MyToken2的实现：

~~~Move
address 0x2 {
module MyToken2 {
    use StarcoinFramework::Token;
    use StarcoinFramework::Account;

    struct MyToken2 has copy, drop, store { }

    public(script) fun init(account: signer) {
        Token::register_token<MyToken2>(&account, 3);
        Account::do_accept_token<MyToken2>(&account);
    }

    public(script) fun mint(account: signer, amount: u128) {
        let token = Token::mint<MyToken2>(&account, amount);
        Account::deposit_to_self<MyToken2>(&account, token)
    }
}
}
~~~

这里需要特别说明的是，MyToken2只是TokenInfo，并不是Token，真正的Token是0x1::Token:Token\<0x2::MyToken2::MyToken2>。

使用Starcoin的Token协议注册并铸造出来的0x1::Token:Token\<0x2::MyToken2::MyToken2>，实际上已经拥有了Token协议中定义的全部功能，非常的简单易用，且功能强大安全。上面只是一个简单的例子，你完全可以根据自己的需求，调用Token协议的函数，设计更复杂、更多样化的FT。



### 3. ERC20 vs Token协议

ERC20是Solidity生态的FT协议，Token协议是Move定义的FT协议。通过前面的原理解析以及深度实战，我们已经清晰的掌握了两种协议的优劣：

* ERC20只是一个规范，并没有可复用的实现，而Token协议面向泛型编程，提供了通用的实现
* ERC20的安全依赖于每种FT自己的实现，容易产生安全隐患，例如进入黑洞、无限增发等等，而Token协议面向资源编程，从VM层面保障了FT的安全可靠
* ERC20协议非常简单，Token协议拥有更丰富、更强大的功能
* ERC20发行的FT只能在当前合约使用，Token协议发行的FT能够在任何合约中使用，并且调用当前合约进行修改
* ERC20发行的FT集中存储在当前合约账户下，Token协议发行的FT能够保存在任何账户下
* Token协议的官方实现，通过形式化证明来进一步保障FT的安全
* 基于Token协议，任何人都需要简单的代码，就能实现一个功能完整且安全的FT
* ERC20没有权限规范，Token协议有非常精细化的权限管理

不管是从Move语言来说，还是从Starcoin的Token协议来说，都非常值得我们思考和借鉴。



## 总结

从现金安全，看ERC20和Move的Token安全，看行业发展。
