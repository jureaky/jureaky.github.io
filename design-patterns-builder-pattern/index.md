# [Design Patterns] Builder Pattern


## 1. Builder Pattern

Builder Pattern is used to create objects, especially when they are rather complex.
We can create objects using so-called **Builder Class**, rather than using constructors. And it's preferred way.


## 2. Benefits of using Builder Pattern

1. Provides a consistent, but flexible way of constructing objects

I usually type in only name and phone number when adding a new friend to my phone address book.
However, there are lots of other fields, which I rarely notice that they even existed while some people might intensely fill most of the fields.

Class representation of a profile might be like below.

```java
public class Profile {
    private String name;
    private String phone;
    private Group group;
    private Address addr;
    private String email;
    private String memo;
}
```

How to handle all those different combination of inputs? :confused:

- 1st Try: Make every possible constructor

```java
public Profile(String name, String phone);
public Profile(String name, String phone, Group group);
/* ... a lot of constructors ... */
public Profile(String name, String phone, Address addr, String memo);
/* ... a lot of constructors ... */
public Profile(String name, String phone, Group group, Address addr, String email, String memo);
```

How many constructors do we need? => $ 2 ^ \text{number of fields} $ !!

This is crazy... And what if we have a class with so many fileds with the same type?? Definitely looks buggy.

- 2nd Try: A single constructor with all fields, then use default/null value

```java
Profile profile = new Profile("John", "1234567", null, null, null, null);
```

This code looks so ugly. And it's likely to misplace values to wrong positions. Not good either.

This is where Builder Pattern comes in.


## 3. Use of Builder Pattern

Here is the implementation.

```java
public class Profile {
    /* ... fields ... */

    // Private Constructor
    // Take as input Builder object
    private Profile(Builder builder) {
        this.name = builder.name;
        this.phone = builder.phone;
        this.group = builder.group;
        this.addr = builder.addr;
        this.email = builder.email;
        this.memo = builder.memo;
    }

    // Builder Class
    public static class Builder {
        private String name;
        private String phone;
        private Group group;
        private Address addr;
        private String email;
        private String memo;

        // Constructor with Mandatory parameters
        public Builder(String name, String phone);

        // Methods to Set Optional Fields
        // Notice that all these methods return 'this(Builder)'

        public Builder group(Group group) {
            this.group = group;
            return this;
        }

        public Builder addr(Address addr) {
            this.addr = addr;
            return this;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public Builder memo(String memo) {
            this.memo = memo;
            return this;
        }

        // Construct a Profile object
        public Profile build() {
            return new Profile(this);
        }
    }

}
```

Client code looks like:

```java
Profile p1 = new Profile.Builder("Kim", "123456")
                .email("kim@email.com")
                .buld();

Profile p2 = new Profile.Builder("Park", "7777777")
                .memo("Genius man")
                .build();
```

Code looks much cleaner and coherent now. :smile:


:point_right: **Thank you for reading my post !** :pray:

:point_right: Please leave a comment if you have any ideas to share ! :notes:

