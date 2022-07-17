# metamask的一些操作 #

> 需要安装[metamask](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn)，确保页面中加载[web3.js](https://cdn.jsdelivr.net/npm/web3@1.7.4/dist/web3.min.js)

## 声明web3 ##

```js
export const web3 = new window.Web3(window.ethereum)
export const eth = web3.eth
```

## 获取账户

```js
function getAddress () {
  eth.getAccounts().then(accounts => {
    if (accounts && accounts.length) {
      let account = accounts[0]
    } else {
      eth.requestAccounts().then(accounts => {
        if (accounts && accounts.length) {
          const account = accounts[0]
        }
      }).catch(err => {
        if (err.code == -32002) {
          Toast('Please unlock your wallet')
        }
      })
    }
  }).catch(err => {
    console.error('getaccounterr..................j', err)
  })
}
```

## 切换链

```js
hecoChainConfig = [{
  chainId: '0x80',
  chainName: 'HECO',
  nativeCurrency: {
    name: 'HT',
    symbol: 'HT',
    decimals: 18
  },
  rpcUrls: ['https://94.74.87.188:8545'],
  blockExplorerUrls: ['https://hecoinfo.com/'],
}]
function switchChain (hecoChainConfig) {
  const ethereum = window.ethereum
  if (!ethereum) return
  if(!data) return
  ethereum.request({
    method: 'wallet_addEthereumChain',
    params: data
  }).then(res => {
    console.log(res)
  }).catch(err => {
    console.error(err)
  })
}
```

## 获取链ID

```js
async function getChainId () {
  const id = await eth.getChainId()
  return id
}
```

## 监听账户切换

```js
ethereum.on('accountsChanged', function (accounts) {
  console.log('accounts changed', accounts)
})
```

## 监听链的切换

```js
ethereum.on('chainChanged', function (chainId) {
  console.log('chainid changed', chainId)
})
```
