## mymall DB에 데이터 넣어보기



![dd](https://user-images.githubusercontent.com/62981623/108092011-9a04c380-70bf-11eb-965c-484a16981eef.jpg)

##### Main.java

```java
package com.megait.main;

import com.megait.model.*;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;


public class Main {
    public static void main(String[] args) {

        EntityManagerFactory factory = Persistence.createEntityManagerFactory("myunit");
        EntityManager em = factory.createEntityManager();
        EntityTransaction transaction = em.getTransaction();
        transaction.begin();

        //Item
        Item item1 = Item.builder()
                .name("만화책")
                .price(10000)
                .stockQuantity(100)
                .build();
        Item item2 = Item.builder()
                .name("잡지")
                .price(20000)
                .stockQuantity(50)
                .build();

        //Member
        Member member1 = Member.builder()
                .email("aaa@gmail.com")
                .password("aaa")
                .type(MemberType.USER)
                .build();

        Member member2 = Member.builder()
                .email("bbb@gmail.com")
                .password("bbb")
                .type(MemberType.USER)
                .build();


        //Orders
        Orders order1 = Orders.builder()
                .member(member1)
                .status(Status.ORDER)
                .build();

        Orders order2 = Orders.builder()
                .member(member2)
                .status(Status.ORDER)
                .build();

        //Delivery
        Delivery delivery1 = Delivery.builder()
                .order(order1)
                .status(DeliveryStatus.READY)
                .build();

        Delivery delivery2 = Delivery.builder()
                .order(order2)
                .status(DeliveryStatus.READY)
                .build();


        em.persist(item1);
        em.persist(item2);
        em.persist(member1);
        em.persist(member2);
        em.persist(delivery1);
        em.persist(delivery2);
        em.persist(order1);
        em.persist(order2);

        transaction.commit();

        em.close();
        factory.close();


    }
}

```



##### persistence.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.2"
             xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">

    <persistence-unit name="myunit">
        <class>com.megait.model.Item</class>
        <class>com.megait.model.Orders</class>
        <class>com.megait.model.Delivery</class>
        <class>com.megait.model.DeliveryStatus</class>
        <class>com.megait.model.Status</class>
        <class>com.megait.model.Member</class>
        <class>com.megait.model.MemberType</class>
        <class>com.megait.model.Album</class>
        <class>com.megait.model.Book</class>
        <class>com.megait.model.Game</class>
        <class>com.megait.model.OrderItem</class>
        <class>com.megait.model.Category</class>
        <class>com.megait.model.Address</class>

        <properties>
            <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
            <property name="javax.persistence.jdbc.user" value="sa"/>
            <property name="javax.persistence.jdbc.password" value=""/>
            <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>

            <!-- JPA가 사용할 방언(언어) -->
            <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>

            <!-- 옵션 -->
            <property name="hibernate.show_sql" value="true"/>
            <property name="hibernate.format_sql" value="true"/>
            <property name="hibernate.use_sql_comments" value="true"/>

            <!-- 테이블 생성 전략 -->
            <property name="hibernate.hbm2ddl.auto" value="create"/>

        </properties>

    </persistence-unit>

</persistence>
```



##### Member.java

```java
package com.megait.model;

import lombok.*;

import javax.persistence.*;
import java.time.LocalDateTime;
import java.util.List;

@Entity
@Setter
@Getter
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Member {

    @Id @GeneratedValue
    private Long id;

    @Column(unique = true)
    private String email;

    private String password;

    @ManyToOne
    @JoinColumn()
    private Address address;

    private LocalDateTime joinedAt;

    private Boolean emailVerified;

    private String emailCheckToken;

    @Enumerated(EnumType.STRING)
    private MemberType type;

    @ManyToMany
    @JoinColumn()
    private List<Item> likes;


    @ManyToMany
    @JoinColumn()
    private List<Orders> orders;


}

```



##### Orders.java

```java
package com.megait.model;

import lombok.*;

import javax.persistence.*;
import java.util.Date;
import java.util.List;

@Entity
@Setter
@Getter
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Orders {

    @Id @GeneratedValue
    private Long id;

    @ManyToOne
    @JoinColumn()
    private Member member;

    @ManyToMany
    @JoinColumn()
    private List<OrderItem> orderItems;

    @ManyToOne
    @JoinColumn()
    private Delivery delivery;

    private Date orderDate;

    @Enumerated(EnumType.STRING)
    private Status status;



}

```



##### Item.java

```java
package com.megait.model;


import lombok.*;

import javax.persistence.*;
import java.util.List;

@Entity
@Setter @Getter @Builder
@AllArgsConstructor @NoArgsConstructor
public class Item {

    @Id
    @GeneratedValue
    private long id;

    private String name;

    private int price;

    private int stockQuantity;

    @ManyToMany
    @JoinColumn()
    private List<Category> categories;

    private String imageUrl;

    private int liked;
}

```



##### OrderItem.java

```java
package com.megait.model;

import lombok.*;

import javax.persistence.*;

@Entity
@Setter
@Getter
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class OrderItem {

    @Id
    @GeneratedValue
    private long id;

    @ManyToOne
    @JoinColumn()
    private  Orders order;

    @ManyToOne
    @JoinColumn()
    private Item item;

    private  int orderPrice;

    private int count;
}

```



##### Delivery.java

```java
package com.megait.model;


import lombok.*;

import javax.persistence.*;

@Entity
@Setter
@Getter
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Delivery {

    @Id
    @GeneratedValue
    private long id;

    @ManyToOne
    @JoinColumn()
    private Orders order;

    @ManyToOne
    @JoinColumn()
    private Address address;

    @Enumerated(EnumType.STRING)
    private DeliveryStatus status;
}

```



##### Category.java

```java
package com.megait.model;

import lombok.*;

import javax.persistence.*;
import java.util.List;

@Entity
@Setter
@Getter
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Category {

    @Id
    @GeneratedValue
    private long id;

    private String name;

    @ManyToMany
    @JoinColumn()
    private List<Item> items;

    @ManyToOne
    @JoinColumn()
    private Category parent;

    @ManyToMany
    @JoinColumn()
    private List<Category> children;
}

```





##### MemberType.java

```java
package com.megait.model;

public enum MemberType {
    ADMIN, USER
}

```



##### Status.java

```java
package com.megait.model;

public enum Status {
    CART,ORDER,CANCEL
}

```



##### DeliveryStatus.java

```java
package com.megait.model;

public enum DeliveryStatus {
    READY, SHIPPING, COMPLETE
}

```



##### Address.java

```java
package com.megait.model;

import lombok.*;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
@Setter
@Getter
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Address {

    @Id
    @GeneratedValue
    private String city;

    private String street;

    private String zipcode;
}

```



##### Book.java

```java
package com.megait.model;

import lombok.*;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
@Setter
@Getter
@Builder
@AllArgsConstructor
@NoArgsConstructor

public class Book {
    @Id
    @GeneratedValue
    private String isbn;
}

```



##### Game.java

```java
package com.megait.model;

import lombok.*;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
@Setter
@Getter
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Game {
    @Id
    @GeneratedValue
    private String title;

    private String publisher;
}

```



##### Album.java

```java
package com.megait.model;

import lombok.*;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
@Setter
@Getter
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Album {
    @Id
    @GeneratedValue
    private String artist;

    private String title;
}

```



