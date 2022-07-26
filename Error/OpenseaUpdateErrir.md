# opensea not showing nft (opensea에서 이미지가 보여지지 않음)

- opensea not showing nft
- opensea에서 이미지가 보여지지 않음

## error
![스크린샷 2022-07-26 오후 10 18 14](https://user-images.githubusercontent.com/88940298/181017566-5a4f30bb-c348-40e3-922f-4fef80d6564c.png)

- tokenuri 및 baseurl까지 작성하고 메타데이터도 규격에 맞춰작성을 하였다.
- 하지만 아무리 메타데이터를 맞추어도 opensea 테스트넷에서 이미지 및 메타데이터가 뜨질 않았다.

```sol

  function tokenURI(uint256 tokenId)
    public
    view
    override(ERC721Upgradeable, ERC721URIStorageUpgradeable)
    returns (string memory)
  {
    string memory base = _baseURI();
    string memory _tokenURI = _tokenURIs[tokenId];
    return string(abi.encodePacked(base, _tokenURI));
  }

```

```sol
  function _baseURI()
    internal
    pure
    override(ERC721Upgradeable)
    returns (string memory)
  {
    return "https://ipfs.io/ipfs/";
  }


```

```js
const metadata = {
  pinataMetadata: {
    name: gg.title
  },
  pinataContent: {
    description: gg.description,
    external_url: "",
    image: gg.img,
    name: gg.title,
    attributes: attributes
  }
};
```

## result

- 오픈씨에서 에러를 찾는 방법을 찾던중

```

https://testnets-api.opensea.io/asset/baobab/<contracthash>/1/validate/
```

- 테스트엣 오픈씨에 뒤애validate를 추가하면 에러를 깊게 확인할수 있었다.
- 에러를 확인해보니 주소가 잘못되어 있는걸 확인하였고 아래러처럼 변경했더니 이미지 및 메타데이터가 잘 뜨는 것을 확인할수 있었다.

```
HTTP 200 OK
Allow: OPTIONS, GET
Content-Type: application/json
Vary: Accept

{
    "valid": false,
    "token_uri": null,
    "errors": [
        "TokenUrl404ResponseException: Received 404 response for: https://ipfs.io/ipfs<hash>"
    ]
}
```

```sol
  function _baseURI()
    internal
    pure
    override(ERC721Upgradeable)
    returns (string memory)
  {
    return "ipfs://";
  }

```
![스크린샷 2022-07-26 오후 10 26 38](https://user-images.githubusercontent.com/88940298/181017492-f23e5817-35fd-4782-a48b-e8e6987466f5.png)


