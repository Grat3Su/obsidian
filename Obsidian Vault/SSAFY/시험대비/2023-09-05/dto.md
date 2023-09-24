DTO는  **Data Transfer Object**의 약자로 계층 간 데이터 전송을 위해 모델 대신 사용되는 객체다.
통신에서 주고받을 데이터와 그것에 접근할 수 있는 getter, setter로 이루어져있다.


``` JAVA
public class Book {

private String isbn;
private String title;
private String author;
private int price;
private String desc;
private String img;

public Book() {}

public Book(String isbn, String title, String author, int price, String desc, String img) {

this.isbn = isbn;
this.title = title;
this.author = author;
this.price = price;
this.desc = desc;
this.img = img;
}

public String getIsbn() {
return isbn;
}

public void setIsbn(String isbn) {
this.isbn = isbn;
}

public String getTitle() {
return title;
} 

public void setTitle(String title) {
this.title = title;
}

public String getAuthor() {
return author;
}

public void setAuthor(String author) {
this.author = author;
}

public int getPrice() {
return price;
}

public void setPrice(int price) {
this.price = price;
}

public String getDesc() {
return desc;
}

public void setDesc(String desc) {
this.desc = desc;
}

public String getImg() {
return img;
}

public void setImg(String img) {
this.img = img;
}

@Override
public String toString() {
return "[" + (isbn != null ? "isbn=" + isbn + ", " : "") + (title != null ? "title=" + title + ", " : "") + (author != null ? "author=" + author + ", " : "") + "price=" + price + ", " + (desc != null ? "desc=" + desc + ", " : "") + (img != null ? "img=" + img : "") + "]";
}
}
```


``` java
package com.ssafy.backend.dto;

  

public class User {

private String id;

private String name;

private String pass;

private String recId;

  

public User() {}

  

public User(String id, String name, String pass, String recId) {

this.id = id;

this.name = name;

this.pass = pass;

this.recId = recId;

}

  

public String getId() {

return id;

}

  

public void setId(String id) {

this.id = id;

}

  

public String getPass() {

return pass;

}

  

public void setPass(String pass) {

this.pass = pass;

}

  

public String getName() {

return name;

}

  

public void setName(String name) {

this.name = name;

}

  

public String getRecId() {

return recId;

}

  

public void setRecId(String recId) {

this.recId = recId;

}

  

@Override

public String toString() {

return "[" + (id != null ? "id=" + id + ", " : "") + (pass != null ? "pass=" + pass + ", " : "") + (name != null ? "name=" + name + ", " : "") + (recId != null ? "recId=" + recId : "") + "]";

}

  

  

}
```