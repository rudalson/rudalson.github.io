---
layout: post
title:  "자신만의 ERC20 Token 만들기"
categories: blockchain
tags: erc20 token
---
`Ethereum`의 `erc20`으로 자신만의 토큰을 만드려고 한다. 여기서는 [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts)의 코드를 사용하였다. 

토큰을 만들기 위해서는 토큰의 기능들 (예를 들면, 전송이나 잔액조회 같은)을  어떻게 할 것인지에 대한 `Solidity` 코드를 작성하여야 하며 이를 `Contract` 작성이라고 한다. 이렇게 만들어진 contract를 이더리움에 올리면 토큰으로서 작동하게 되는데 한번 올린 다음에는 수정이 쉽지 않기 때문에 초기에 충분히 테스트 하여야 한다.

이러한 이유로 `OpenZeppelin`에서는 (아마도 다른것도 마찬가지일 것 같지만) 기존에 충분히 테스트 되어진 contract 코드를 제공하며 이를 가져다 일부만 변경한 다음에 적용하면 토큰을 바로 사용할 수 있다.

# Instructions
Solidity를 위한 IDE는 [Remix IDE](http://remix.ethereum.org)를 사용한다.

1. [openzeppelin-contracts github](https://github.com/OpenZeppelin/openzeppelin-contracts)에서 [contracts/token/ERC20](https://github.com/OpenZeppelin/openzeppelin-contracts/tree/master/contracts/token/ERC20) 디렉토리로 간다.
![OpenZeppelin]({{ site.url }}/assets/2019/2019-10-22.openzepplin-contracts-erc20.png)

1. `ERC20.sol`, `ERC20Burnable.sol`, `ERC20Detailed.sol`, `IERC20.sol`, `SafeERC20.sol` 을 가져다 IDE에 등록시킨다.
ERC20.sol 에서 현재 소스의 solidity 버전을 확인할 수 있다. 그리고 import 파일중에서 다른 디렉토리에 있는 파일들도 모두 찾아서 구해준다. 아래의 경우 `Context.sol`과 `SafeMath.sol`도 각각의 디렉토리로 들어 가서 모두 구해온다. 
    ```
    pragma solidity ^0.5.0;

    import "../../GSN/Context.sol";
    import "./IERC20.sol";
    import "../../math/SafeMath.sol";
    ```

1. 이렇게 모든 파일들을 구해서 Remix IDE에 생성한 후 import의 디렉토리도 모두 `“./”` 으로 변경한다. 가령 위의 ERC20.sol은 아래처럼 될 것이다.
    ```
    import "./Context.sol";
    import "./IERC20.sol";
    import "./SafeMath.sol";
    ```

1. [contracts/examples](https://github.com/OpenZeppelin/openzeppelin-contracts/tree/master/contracts/examples) 디렉토리로 가서 `SimpleToken.sol` 을 구해와서 몇가지를 수정해 준다. import 디렉토리도 모두 “./” 으로 변경한다. 코드상에서 `Context`와 `ERC20Burnable`을 추가시켜준다. 그리고 토큰명과 토큰의 단위가 될 축약어를 지정해준다. 아래 예제에서는 `BusToken`과 `BTK`로 해주었다. 그러면 아래와 같은 코드가 될 것이다. 참고로, ERC20Burnable에는 토큰의 소각기능이 포함되어 있다. 이런 것들이 필요한 경우 몇몇가지를 상속의 형태로 가져오면 된다.
    ```
    pragma solidity ^0.5.0;

    import "./Context.sol";
    import "./ERC20.sol";
    import "./ERC20Detailed.sol";
    import "./ERC20Burnable.sol";

    contract SimpleToken is Context, ERC20, ERC20Detailed, ERC20Burnable {
        
        constructor () public ERC20Detailed("BusToken", "BTK", 18) {
            _mint(_msgSender(), 10000 * (10 ** uint256(decimals())));
        }
    }
    ```

5. 최종 나온 코드의 파일들은 아래와 같다.
    * ERC20Burnable.sol
    * IERC20.sol
    * ERC20Detailed.sol
    * SafeMath.sol
    * Context.sol
    * SimpleToken.sol
    * ERC20.sol

6. REMIX IDE의 `Compile`탭에서 `compiler version` 을 `0.5.0` 으로 맞춰준 후 compile를 해준다.
![Remix compiler tab]({{ site.url }}/assets/2019/2019-10-22.remix-compile-tab.png)

7. `Run`탭에서 `Environment`를 `JavaScriptVM`으로 맞추고 `SimpleToken`을 `Deploy`한다. 그러면 아래에 Deployed 된 기능들이 생겨남을 알 수 있으며 이를 테스트 할 수 있다. JavaScriptVM에서 deploy 한다는 것은 이더리움에 아직 올린것이 아니라 local 에서 vm을 구성하여 만든 것이고 이를 통해 기능 테스트를 미리 해볼 수 있다.
![Remix run tab]({{ site.url }}/assets/2019/2019-10-22.remix-run-tab.png)

8. 테스트에 문제가 없다면 실제 이더리움넷에 deploy 하면 된다. Environment를 `Injected Web3 Robsten`(테스트넷)으로 해서 Deploy한다. 실제 토큰을 생성하기 위해서는 전체 수량을 가질 최초의 지갑을 자신이 접근가능한 주소로 한다.
![Web3 deploy]({{ site.url }}/assets/2019/2019-10-22.web-deploy.png)
여기서 주의사항으로는 토큰생성에 대한 contract 수행도 `Ether Gas fee` 가 발생하기 때문에 테스트넷용 이더넷을 확보한 뒤 contract deploy를 한다. deploy가 성공하면 아래와 같이 `etherscan`에서 확인 가능하다.(테스트넷은 간헐적으로 reset 되므로 일정시간이 지나면 사라질 수도 있다)
![Etherscan contract]({{ site.url }}/assets/2019/2019-10-22.contract-etherscan.png)

9. 이후 wallet에서 확인해 보면 토큰을 확인 할 수 있다.
![Created Token]({{ site.url }}/assets/2019/2019-10-22.token-in-wallet.png)

