# Ethereum accounts package

## Account

- 모든 트랜잭션의 실행 주체
- 이더리움 플랫폼에서의 기본 단위 - EOA와 CA로 구분된다



### EOA (Externally Owned Account)

- 이더리움 사용자 어카운트, 외부 소유 어카운트로 불린다
- 사용자가 자신의 private key로 관리
  - 만약, private key를 분실하면 영영 계정을 분실하는 것과 동일

### CA (Contract Account)

- 스마트 컨트랙트
- EOA나 다른 CA의 메시지를 받은 후 내부의 스마트 컨트랙트 코드를 실행한 후, 새로운 컨트랙트를 생성하거나 다른 메시지를 읽거나 보낼 수 있음
- 내부 저장 공간에 데이터를 저장할 수도 있음
- EOA나 다른 CA에 의해서만 작동되고, 자기 자신이 새로운 트랜잭션을 실행할 수는 없음



- 어카운트 주소 (accounts/accounts.go 와 common/types.go)

```Go
package accounts

// Account represents an Ethereum account located at a specific location defined
// by the optional URL field.
type Account struct {
	Address common.Address `json:"address"` // Ethereum account address derived from the key // key로부터 얻어진 Ethereum account 주소
	URL     URL            `json:"url"`     // Optional resource locator within a backend 
}
```

> - Address : 20 byte 고정 배열
> - URL : 해당 Address의 위치 (optional)

```go
const (
	// AddressLength is the expected length of the address
	AddressLength = 20 // Address 길이
)
// Address represents the 20 byte address of an Ethereum account.
type Address [AddressLength]byte // Ethereum account의 20 byte 고정 배열
```



- 어카운트 정보 (core/state/state_object.go)

```go
// Account is the Ethereum consensus representation of accounts.
// These objects are stored in the main account trie.
type Account struct {
	Nonce    uint64
	Balance  *big.Int
	Root     common.Hash // merkle root of the storage trie
	CodeHash []byte
}
```

> - Nonce : 해당 Account로부터 보내진 트랜잭션의 개수, 0으로 시작하는 값
> - Balance : 해당 Account의 Ether 잔액 (단위는 Wei)
> - Root : 해당 Account가 저장될 머클 패트리시아 트리의 루트 노드의 해시값 (Keccak256 해시)
> - CodeHash : 해당 Account의 스마트 컨트랙트 바이트 코드의 해시값
>   - nil 인 경우 : 이 Account는 EOA
>   - nil이 아닌 경우 : 이 Account는 CA 



[Reference]

박재현, 오재훈, 박혜영. (2018). **코어 이더리움 프로그래밍**. 경기도 파주시: 주식회사 제이펍.