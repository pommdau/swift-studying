## [2022-05-20_Qiita Night_Swift6がもたらす開発者体験を予測しよう！](https://increments.connpass.com/event/246181/)

### [偏見と妄想で語るスクリプト言語としての Swift / Swift as a Scripting Language](https://speakerdeck.com/lovee/swift-as-a-scripting-language)
- Swiftをスクリプト言語のように使っているという話。
    - e.g. Danger-Swiftの設定ファイル？
- 今年のWWDCでSwiftからスクリプトがより簡単に呼び出せるとの嬉しいよね、という願望。
    - 私も社内ツールで使ってるけれど、確かににそれは思う。

### [【Qiita Night】新卒エンジニアによるSwift6与太予想](https://speakerdeck.com/eiji127/qiita-night-xin-zu-enzinianiyoruswift6yu-tai-yu-xiang)
- プロトコルとリバースジェネリクスは`Heart of Swift`の通り。
- Concurrency/ARKit/Xcode Cloud/サーバサイド etcの雑多な紹介。
- 実際にサーバサイドSwiftはAmazon Prime Videoで使われているとのこと。
    - [iOSアプリをSwiftで統一するとは](https://zenn.dev/masatsch/articles/102ea67a7de671#ios%E3%82%A2%E3%83%97%E3%83%AA%E3%82%92swift%E3%81%A7%E7%B5%B1%E4%B8%80%E3%81%99%E3%82%8B%E3%81%A8%E3%81%AF)

### [Swift Concurrencyによる安全で快適な非同期処理](https://speakerdeck.com/tattn/qiitanightswift6)
- 別の所で聞いた話なので割愛。

### メモ
- 「Swiftの嫌いな所あります？」と質問されて、無いよね〜というくだりは面白かった。皆Swiftが好きすぎる。
- Swift Concurrencyは簡単な処理に関して軽く触っているだけなので、もう少し詰めていきたい。
    - Combineも以下のように書き直せるハズ。
- 簡単な処理ならいいのだけれど、例えばDBの書き込みのメインスレッドが絡んでくると、よく分からなくなってくる…。学習不足っぽい気はする。

```swift
    func request<Request>(with request: Request) -> AnyPublisher<Request.Response, APIServiceError> where Request: APIRequestType {
        
        guard let pathURL = URL(string: request.path, relativeTo: URL(string: baseURLString)) else {
            return Fail(error: APIServiceError.invalidURL).eraseToAnyPublisher()
        }
        
        var urlComponents = URLComponents(url: pathURL, resolvingAgainstBaseURL: true)!
        urlComponents.queryItems = request.queryItems
        var request = URLRequest(url: urlComponents.url!)
        request.addValue("application/json", forHTTPHeaderField: "Accept")
        
        let decorder = JSONDecoder()
        decorder.keyDecodingStrategy = .convertFromSnakeCase
        return URLSession.shared.dataTaskPublisher(for: request)
            .map { data, urlResponse in data }  // mapでレスポンスデータのストリームに変換
            .mapError { _ in APIServiceError.responseError }  // エラーが起きたらAPIServiceError.responseErrorを返す
            .decode(type: Request.Response.self, decoder: decorder)  // デコード
            .mapError({ (error) -> APIServiceError in
                APIServiceError.parseError(error)
            })
            .receive(on: RunLoop.main)  // ストリームをメインスレッドに流れるように変換
            .eraseToAnyPublisher()
    }
```

```swift    
    // Concurrencyで書くと以下の通り?(動作未検証)
    
    func request<Request>(with request: Request) async throws -> Request.Response where Request: APIRequestType {
        guard let pathURL = URL(string: request.path, relativeTo: URL(string: baseURLString)) else {
            throw APIServiceError.invalidURL
        }
        
        var urlComponents = URLComponents(url: pathURL, resolvingAgainstBaseURL: true)!
        urlComponents.queryItems = request.queryItems
        var request = URLRequest(url: urlComponents.url!)
        request.addValue("application/json", forHTTPHeaderField: "Accept")
        
        let decorder = JSONDecoder()
        decorder.keyDecodingStrategy = .convertFromSnakeCase
        
        let (data, response) = try await URLSession.shared.data(for: request)
        guard (response as? HTTPURLResponse)?.statusCode == 200 else { throw APIServiceError.responseError }
        do {
            return try decorder.decode(Request.Response.self, from: data)
        } catch {
            throw APIServiceError.parseError(error)
        }
    }
```
