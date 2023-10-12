# JS로 Queue 구현하기

JavaScript는 내장된 queue가 없기 때문에 `shift`, `push`를 사용. 

`shift`메소드 경우 복잡도가 최악일 경우 **O(n)** 의 복잡도를 가짐  
(push / pop은 O(1))  
→ 시간 초과가 날 가능성 높음

### Linked List를 활용한 Queue 구현
```js
class Node {
    constructor(data) {
        this.data = data;
        this.next = null;
    }
}

// 큐 클래스
class Queue {
    constructor() {
        this.head = null; // 제일 앞 노드
        this.rear = null; // 제일 뒤 노드
        this.length = 0;  // 노드의 길이
    }

    enqueue(data) {
        // 노드 추가
        const node = new Node(data); // data를 가진 node를 만들어줌
        if(!this.head) {
            // 헤드가 없을 경우 head를 해당 노드로
            this.head = node;
        } else {
            this.rear.next = node;  // 아닐 경우 마지막의 다음 노드로
        }
        this.rear = node;  // 마지막을 해당 노드로 함
        this.length++;
    }

    dequeue() {
        // 노드 삭제
        if(!this.head) {
            // 헤드가 없으면 비어있으니까 false 반환
            return false;
        }
        const data = this.head.data;  // head를 head의 다음 것으로 바꿔주고 뺀 data를 return
        this.head = this.head.next;
        this.length--;

        return data;
    }
    front() {
        // head 반환
        return this.head && this.head.data;  // head가 있을 경우 head의 data를 반환
    }

    getQueue() {
        // 큐의 모든 원소를 반환
        if(!this.head) return false;
        let node = this.head;
        const array = [];
        while(node) {
            // node가 없을 때까지 array에 추가한 후 반환
            array.push(node.data);
            node = node.next;
        }
        return array;
    }
}
```

```js
const queue = new Queue();
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);
queue.enqueue(4);
queue.dequeue();
console.log(queue.front());
console.log(queue.dequeue());
console.log(queue.getQueue());
```

→

```powershell
2
2
[3, 4]
```