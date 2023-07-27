# Redux原理

![image-20230725185347426](/Users/gaga/Desktop/knowledge/Redux原理.assets/image-20230725185347426.png)

**图片解读：**网页信息被用户浏览，用户作为主体触发页面产生Action，Action传到了Redux创建的Store，Reducer是各种的规则它负责解析Action改变State。Store订阅了Subscribe，Subscribe负责监听State，从而改变页面的数据

```
import { createStore } from "redux";
let recordState;
const initialState = [];
const reducer = function (state = initialState, action) {
  recordState = state;
  switch (action.type) {
    case "addBook":
      return [
        ...state,
        {
          bookId: action.info.bookId,
          bookName: `<<${action.info.bookName}>>`
        }
      ]
    case "delBook":
      return state.filter(book => book.bookId != action.info.bookId)
    default:
      return [
        ...state
      ];
      break;
  }
}
const store = createStore(reducer);
const root = document.getElementById("app");
const addBook = document.getElementById("addBook");
const delBook = document.getElementById("delBook");
const bookList = document.getElementById("bookList");

const addBookBtn = document.createElement("button");
const bookNameInput = document.createElement("input");
const delBookBtn = document.createElement("button");
const bookIdInput = document.createElement('input');

addBookBtn.innerText = "ADD BOOK";
delBookBtn.innerText = "DEL BOOK";

addBookBtn.addEventListener("click", addBookFn);
delBookBtn.addEventListener("click", delBookFn);
function* generateID() {
  let id = 0;
  while (true) {
    yield id++;
  }
}
const generateId = generateID();
const genBookId = () => generateId.next().value;

function addBookFn() {
  const bookName = bookNameInput.value;
  if (bookName) {
    console.log(bookName);
    const bookId = genBookId();
    bookNameInput.value = ""
    const action = {
      type: "addBook",
      info: {
        bookId: bookId,
        bookName: bookName
      }
    }
    store.dispatch(action)
  }
}
function delBookFn() {
  const bookId = bookIdInput.value;
  console.log(bookId);
  if (bookId) {
    bookIdInput.value = "";
    const action = {
      type: "delBook",
      info: {
        bookId: bookId
      }
    }
    store.dispatch(action)
  }
}
addBook.appendChild(bookNameInput);
addBook.appendChild(addBookBtn);
delBook.appendChild(bookIdInput);
delBook.appendChild(delBookBtn);

const showState = store.subscribe(() => {
  console.log(store.getState());
});
const showNewList = store.subscribe(() => {
  const currentState = store.getState() 
  if (currentState.length !== recordState.length) {
    bookList.innerText = ""
    currentState.forEach(element => {
      bookList.appendChild(createBookList(element))
    })
  }
})

function createBookList(info) {
  const element = document.createElement("li");
  element.innerText = `BookID: ${info.bookId} BookNAME: ${info.bookName}`
  return element;
}
```

