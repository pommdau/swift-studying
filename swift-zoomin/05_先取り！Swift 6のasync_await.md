## [Swift Zoomin' \#5](https://www.youtube.com/watch?v=OZN_YFZy770)
- [Swift Zoomin' \#5](https://www.youtube.com/watch?v=OZN_YFZy770)
- [先取り！ Swift 6 の async/await](https://speakerdeck.com/koher/await)

## async/await について
### 練習問題01

```swift
// サーバーから JSON を取得し、
// User インスタンスをデコードする
// 非同期関数 fetchUser を完成させて下さい。
//
// サーバーから JSON を取得するには
// download 関数を用います。
// download の実装は擬似的なものですが
// 変更せずにそのまま利用して下さい。
//
// なお、通信やデコードに起因するエラーは
// 起こらないものとします。

import Foundation
import FoundationNetworking

struct User: Identifiable, Codable {
    typealias ID = Int
    let id: ID
    var name: String
    var thumbnailURL: URL
}
func download(from url: URL, completion: @escaping (Data) -> Void) {
    let data: Data = try! Data(contentsOf: url)
    completion(data)
}

func fetchUser(for id: User.ID, completion: @escaping (User) -> Void) {
    let url: URL = URL(string: "https://koherent.org/async-await-challenge/api/user?id=\(id.description)")!
    // 🚧 ここを実装する
    download(from: url) { data in 
        let user = try! JSONDecoder().decode(User.self, from: data)
        completion(user)
    }
}

fetchUser(for: 123) { user in
    print(user.name)
}
```

```swift
// サーバーから JSON を取得し、
// User インスタンスをデコードする
// 非同期関数 fetchUser を完成させて下さい。
//
// サーバーから JSON を取得するには
// download 関数を用います。
// download の実装は擬似的なものですが
// 変更せずにそのまま利用して下さい。
//
// なお、通信やデコードに起因するエラーは
// 起こらないものとします。

import Foundation
import FoundationNetworking

struct User: Identifiable, Codable {
    typealias ID = Int
    let id: ID
    var name: String
    var thumbnailURL: URL
}
func download(from url: URL) async -> Data {
    let data: Data = try! Data(contentsOf: url)
    return data
}

func fetchUser(for id: User.ID) async -> User {
    let url: URL = URL(string: "https://koherent.org/async-await-challenge/api/user?id=\(id.description)")!
    // 🚧 ここを実装する
    let data = await download(from: url)
    let user = try! JSONDecoder().decode(User.self, from: data)
    return user
}

do {
    let user = await fetchUser(for: 123)
    print(user.name)
}
```

- resultをswitchではなく以下のように書くと、複数のエラーに対応しやすくて良い。

![](media/16518423682162/16518438102363.jpg)

### 練習問題02

```swift
// サーバーから JSON を取得し、
// User インスタンスをデコードする
// 非同期関数 fetchUser を完成させて下さい。

import Foundation
import FoundationNetworking

struct User: Identifiable, Codable {
    typealias ID = Int
    let id: ID
    var name: String
    var thumbnailURL: URL
}
func download(from url: URL, completion: @escaping (Result<Data, Error>) -> Void) {
    do {
        let data: Data = try Data(contentsOf: url)
        completion(.success(data))
    } catch {
        completion(.failure(error))
    }
}

func fetchUser(for id: User.ID, completion: @escaping (Result<User, Error>) -> Void) {
    let url: URL = URL(string: "https://koherent.org/async-await-challenge/api/user?id=\(id.description)")!
    // 🚧 ここを実装する
    download(from: url) { result in 
        do {
            let data: Data = try result.get()
            let user = try! JSONDecoder().decode(User.self, from: data)
            completion(.success(user))
        } catch {
            completion(.failure(error))
            print("download error")
        }
    }
}

fetchUser(for: 123) { user in
    do {
        print(try user.get().name)
    } catch {
        print(error)
    }
}
```

- throwsの伝搬で、fetchUserでゴニョゴニョresultの処理をしなくて済む

```swift
// サーバーから JSON を取得し、
// User インスタンスをデコードする
// 非同期関数 fetchUser を完成させて下さい。

import Foundation
import FoundationNetworking

struct User: Identifiable, Codable {
    typealias ID = Int
    let id: ID
    var name: String
    var thumbnailURL: URL
}
func download(from url: URL) async throws -> Data {
    let data: Data = try Data(contentsOf: url)
    return data
}

func fetchUser(for id: User.ID) async throws -> User {
    let url: URL = URL(string: "https://koherent.org/async-await-challenge/api/user?id=\(id.description)")!
    // 🚧 ここを実装する
    let data: Data = try await download(from: url)
    let user = try JSONDecoder().decode(User.self, from: data)
    return user
}

do {
    do {
        let user: User = try await fetchUser(for: 123)
        print(user.name)
    } catch {
        print(error)
    }
}
```

### 練習問題03

```swift
// サーバーから JSON を取得し、
// User インスタンスをデコードし、
// それに加えて User が保持する thumbnailURL から
// サムネイル画像をダウンロードする非同期関数
// fetchUserWithThumbnail を完成させて下さい。

import Foundation
import FoundationNetworking

struct User: Identifiable, Codable {
    typealias ID = Int
    let id: ID
    var name: String
    var thumbnailURL: URL
}
func download(from url: URL, completion: @escaping (Result<Data, Error>) -> Void) {
    do {
        let data: Data = try Data(contentsOf: url)
        completion(.success(data))
    } catch {
        completion(.failure(error))
    }
}

func fetchUserWithThumbnail(for id: User.ID, completion: @escaping (Result<(user: User, thumbnail: Data), Error>) -> Void) {
    let url: URL = URL(string: "https://koherent.org/async-await-challenge/api/user?id=\(id.description)")!
    // 🚧 ここを実装する
    download(from: url) { result in 
        do {
            let data: Data = try result.get()
            let user = try JSONDecoder().decode(User.self, from: data)

            download(from: user.thumbnailURL) { result in 
                do {
                    let thumbnail: Data = try result.get()
                    completion(.success((user, thumbnail)))
                } catch {
                    completion(.failure(error))
                    print("thumbnail download error")
                }
            }            
        } catch {
            completion(.failure(error))
            print("download error")
        }
    }
}

fetchUserWithThumbnail(for: 123) { result in
    do {
        let (user, thumbnail) = try result.get()
        print(user.name, thumbnail.count)
    } catch {
        print(error)
    }
}
```

- ネストが浅くなるだけでなく、エラーハンドリングの可読性が高い。

```swift
// サーバーから JSON を取得し、
// User インスタンスをデコードし、
// それに加えて User が保持する thumbnailURL から
// サムネイル画像をダウンロードする非同期関数
// fetchUserWithThumbnail を完成させて下さい。

import Foundation
import FoundationNetworking

struct User: Identifiable, Codable {
    typealias ID = Int
    let id: ID
    var name: String
    var thumbnailURL: URL
}
func download(from url: URL) async throws -> Data {
    let data: Data = try Data(contentsOf: url)
    return data
}

func fetchUserWithThumbnail(for id: User.ID) async throws -> (user: User, thumbnail: Data) {
    let url: URL = URL(string: "https://koherent.org/async-await-challenge/api/user?id=\(id.description)")!
    // 🚧 ここを実装する

    let userData: Data = try await download(from: url)
    let user = try JSONDecoder().decode(User.self, from: userData)
    let thumbnail: Data = try await download(from: user.thumbnailURL)

    return (user, thumbnail)
}

do {
    do {
        let (user, thumbnail) = try await fetchUserWithThumbnail(for: 123)
        print(user.name, thumbnail.count)
    } catch {
        print(error)
    }
}
```

### 練習問題04

![](media/16518423682162/16518459148660.jpg)

```swift
// サーバーから User とその Article 最新 10 件の JSON を取得し、
// それらを返す非同期関数 fetchUserWithArticles を実装して下さい。
// ただし、 User と Article の JSON は並行して取得するものとし、
// User と Artcile の取得には fetchUser および
// fetchArticles を用いるものとします。

import Foundation
import FoundationNetworking

struct User: Identifiable, Codable {
    typealias ID = Int
    let id: ID
    var name: String
    var thumbnailURL: URL
}
struct Article: Identifiable, Codable {
    typealias ID = Int
    let id: ID
    var title: String
}
func fetchUser(for id: User.ID) async throws -> User {
    let url: URL = URL(string: "https://koherent.org/async-await-challenge/api/user?id=\(id.description)")!
    let data: Data = try Data(contentsOf: url)
    let user: User = try JSONDecoder().decode(User.self, from: data)
    return user
}
func fetchArticles(for userID: User.ID, limit: Int) async throws -> [Article] {
    let url: URL = URL(string: "https://koherent.org/async-await-challenge/api/articles?userID=\(userID.description)")!
    let data: Data = try Data(contentsOf: url)
    let articles: [Article] = try JSONDecoder().decode([Article].self, from: data)
    return articles
}

func fetchUserWithArticles(for id: User.ID, limit: Int) async throws -> (user: User, articles: [Article]) {
    // 🚧 ここを実装する
    async let user = fetchUser(for: id)
    async let articles = fetchArticles(for: id, limit: limit)
    return try await (user, articles)
}

do {
    do {
        let (user, articles) = try await fetchUserWithArticles(for: 123, limit: 10)
        print(user, articles)
    } catch {
        print(error)
    }
}
```

### 練習問題05

![](media/16518423682162/16519034443965.jpg)

- なんかエラーでる…
- `@asyncHandler`は廃止された？
    - https://discord.com/channels/291054398077927425/291054454793306112/850782177478049832

```swift
// ViewController の reloadUserButton が押されたときに
// fetchUser 関数を使ってサーバーから User を取得し、
// userNameLabel.text に取得したユーザーの name を設定するように
// onReloadUserButtonPressed メソッドを完成させて下さい。

import Foundation
import FoundationNetworking

class UIViewController {}
final class UIButton {}
final class UILabel {
    var text: String?
}

struct User: Identifiable, Codable {
    typealias ID = Int
    let id: ID
    var name: String
    var thumbnailURL: URL
}
func fetchUser(for id: User.ID) async throws -> User {
    let url: URL = URL(string: "https://koherent.org/async-await-challenge/api/user?id=\(id.description)")!
    let data: Data = try Data(contentsOf: url)
    let user: User = try JSONDecoder().decode(User.self, from: data)
    return user
}

final class ViewController: UIViewController {
    let reloadUserButton: UIButton = .init()
    let userNameLabel: UILabel = .init()
    let userID: User.ID
    init(userID: User.ID) { self.userID = userID }

    // 🚧 このメソッドを完成させる
    @asyncHandler
    func onReloadUserButtonPressed(_ sender: UIButton) {
        if let user = try? await fetchUser(for: userID) {
            print(user.name)
            userNameLabel.text = user.name
        }    
    }
}

let viewController: ViewController = .init(userID: 123)
viewController.onReloadUserButtonPressed(viewController.reloadUserButton)
```

### 練習問題06

![](media/16518423682162/16519080189020.jpg)

- エラーとなるけど雰囲気をつかむ

```swift
// ViewController の cancelReloadingUser ボタンを押すと
// reloadUser ボタンで実行されている User のリロードを
// キャンセルするように ViewController の実装を完成させて下さい。

import Foundation
import FoundationNetworking

class UIViewController {}
final class UIButton {}
final class UILabel {
    var text: String?
}

struct User: Identifiable, Codable {
    typealias ID = Int
    let id: ID
    var name: String
    var thumbnailURL: URL
}
func fetchUser(for id: User.ID) async throws -> User {
    let url: URL = URL(string: "https://koherent.org/async-await-challenge/api/user?id=\(id.description)")!
    let data: Data = try Data(contentsOf: url)
    let user: User = try JSONDecoder().decode(User.self, from: data)
    return user
}

// 🚧 このクラスを完成させる
final class ViewController: UIViewController {
    let reloadUserButton: UIButton = .init()
    let cancelReloadingUserButton: UIButton = .init()
    let userNameLabel: UILabel = .init()
    let userID: User.ID
    init(userID: User.ID) { self.userID = userID }

    private var userReloadingHandle: Task.Handle<Void>?

    func onReloadUserButtonPressed(_ sender: UIButton) {
        userReloadingHandle = Task.runDetached { [self] in
            if let user = try? await fetchUser(for: userID) {
                userNameLabel.text = user.name
            }
        }        
    }

    func onCancelReloadingUserButtonPressed(_ sender: UIButton) {
        userReloadingHandle?.cancel()
    }
}

let viewController: ViewController = .init(userID: 123)
viewController.onReloadUserButtonPressed(viewController.reloadUserButton)
viewController.onCancelReloadingUserButtonPressed(viewController.cancelReloadingUserButton)
```

### 練習問題07

![](media/16518423682162/16519083823761.jpg)

```swift
// コールバックで結果を受け取る非同期関数 download を使って、
// async で結果を返す非同期関数 download を実装して下さい。

import Foundation
import FoundationNetworking

func download(from url: URL, completion: @escaping (Result<Data, Error>) -> Void) {
    do {
        let data: Data = try Data(contentsOf: url)
        completion(.success(data))
    } catch {
        completion(.failure(error))
    }
}

// 🚧 ここを実装する
func download(from url: URL) async throws -> Data {
    try await withUnsafeThrowingContinuation { continuation in 
        download(from: url) { result in 
            continuation.resume(with: result)
        }
    }
}

do {
    do {
        let url: URL = URL(string: "https://koherent.org/async-await-challenge/api/user?id=123")!
        let data: Data = try await download(from: url)
        print(String(data: data, encoding: .utf8)!)
    } catch {
        print(error)
    }
}
```
