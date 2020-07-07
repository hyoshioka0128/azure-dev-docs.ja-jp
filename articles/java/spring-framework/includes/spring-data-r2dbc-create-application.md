---
author: judubois
ms.date: 05/06/2020
ms.author: judubois
ms.openlocfilehash: e1bd45413368abe253ff4ac7733bbdcd3d0a4cc3
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507512"
---
次のコードを使用し、`DemoApplication` クラスの横に新しい `Todo` Java クラスを作成します。

```java
package com.example.demo;

import org.springframework.data.annotation.Id;

public class Todo {

    public Todo() {
    }

    public Todo(String description, String details, boolean done) {
        this.description = description;
        this.details = details;
        this.done = done;
    }

    @Id
    private Long id;

    private String description;

    private String details;

    private boolean done;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getDetails() {
        return details;
    }

    public void setDetails(String details) {
        this.details = details;
    }

    public boolean isDone() {
        return done;
    }

    public void setDone(boolean done) {
        this.done = done;
    }
}
```

このクラスは、先ほど作成した `todo` テーブルにマップされるドメイン モデルです。

このクラスを管理するには、リポジトリが必要です。 次のコードを使用し、同じパッケージ内に新しい `TodoRepository` インターフェイスを定義します。

```java
package com.example.demo;

import org.springframework.data.repository.reactive.ReactiveCrudRepository;

public interface TodoRepository extends ReactiveCrudRepository<Todo, Long> {
}
```

このリポジトリは、Spring Data R2DBC によって管理されるリアクティブ リポジトリです。

データを格納および取得できるコントローラーを作成して、アプリケーションを完成させます。 同じパッケージに `TodoController` クラスを実装し、次のコードを追加します。

```java
package com.example.demo;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@RestController
@RequestMapping("/")
public class TodoController {

    private final TodoRepository todoRepository;

    public TodoController(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    @PostMapping("/")
    @ResponseStatus(HttpStatus.CREATED)
    public Mono<Todo> createTodo(@RequestBody Todo todo) {
        return todoRepository.save(todo);
    }

    @GetMapping("/")
    public Flux<Todo> getTodos() {
        return todoRepository.findAll();
    }
}
```

最後に、次のコマンドを使用して、アプリケーションを停止して再起動します。

```bash
./mvnw spring-boot:run
```

## <a name="test-the-application"></a>アプリケーションをテストする

アプリケーションをテストするには、cURL を使用します。

まず、次のコマンドを使用して、データベースに新しい "todo" 項目を作成します。

```bash
curl --header "Content-Type: application/json" \
    --request POST \
    --data '{"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done": "true"}' \
    http://127.0.0.1:8080
```

このコマンドからは、ここに示すように、作成された項目が返されるはずです。

```json
{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}
```

次に、次のコマンドを使用し、新しい cURL 要求を使用してデータを取得します。

```bash
curl http://127.0.0.1:8080
```

このコマンドからは、ここに示すように、作成した項目を含む "todo" 項目の一覧が返されます。

```json
[{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}]
```
