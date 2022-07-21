## 什么是Omit？

Omit是ts官方在[3.5版本](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-5.html)提出一个新的类型关键词，对于它的解释官方的原话是 creates a new type with some properties dropped from the original，意思是以一个类型为基础支持剔除某些属性，并返回一个新的类型。下面讲讲具体的使用方法：

``` typescript
 type Person = {
    name: string;
    age:number;
    location: string;
}

type PersonWithoutLocation = Omit<Person, 'location'>;

// 上面我们用Omit关键词定义了一个新的类型PersonWithoutLocation，它的实际属性如下：

 type PersonWithoutLocation = {
    name:string;
    age:number;
 }    
```

正如上面的代码所示，我们可以看出来Omit有两个参数，第一个参数传入的就是原类型，第二个参数为你想要在原类型中想要剔除的属性。

了解过Omit的用法之后，我们可以深入了解一下内部实现的原理，这样我们就能可以更好的在实际中去运用它。


``` typescript
    type Omit<T, K extends keyof any> = type Pick<T, 
    Exclude<keyof T, K>>
```


看到上面的等式之后，第一眼看起来好像不太好理解，我们一步一步来理解上面的等式。

### keyof

首先，我们要理解 keyof 的定义，keyof 运算符诞生于[2.1版本](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-1.html)，它的作用是得到一个类型中所有的键，返回的是一个联合类型。下面是它的举例：

``` typescript
    interface Person {
        name:string;
        age:number;
        location:string;
    }
    
    type k1 = keyof Person; // name | age | location
    type k2 = keyof Person[]; // length | push | pop | concat | ...
    type k3 = keyof { [x:string]: Person }; // string
```

了解完 keyof 的作用后，接下开来理解 Exclude 关键词的作用。

### Exclude

Exclude 关键词是[2.8版本](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html)提出的，它的作用是将两个类型A和B进行比较，返回A类型中独有的类型。举例如下：
``` typescript
    type person = Exclude<"a" | "b" | "c" | "d", "a" | "c" | "f">;
    // "b" | "d"
```

上面的例子中，我们可以清晰的理解Exclude的用法。了解完 keyof 和 exclude 两个关键词后，我们再了解一下 Pick 关键词。

### Pick

Pick 关键词是[2.1版本](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-1.html)提出的关键词，Pick< Type, Keys >,作用就是将Type类型中的Keys拿到并给它返回出来，举例如下：
``` typescript
    type Person = {
        name:string;
        age:string;
        location:string;
    }

    const pickDataType = Pick(Person, "name", "age");
    // { name:string; age:string; }
```


了解完上述的关键词后，我们就可以去理解开头列出来的等式了，我们可以试着去推导一下。

``` typescript
    type Person = {
    name: string;
    age:number;
    location: string;
}

    type PersonWithoutLocation = Omit<Person, "location">;
    // 推导
    type PersonWithoutLocation = Pick<Person, Exclude<keyof Person, "location">>;
    // 推导
    type PersonWithoutLocation = Pick<Person, Exclude<("name","age","location"), "location">>;
    // 推导
    type PersonWithoutLocation = Pick<Person,"name","age">;
    // 推导
    type PersonWithoutLocation = {
        name: string;
        age: numbrt;
    }
    // 经过我们的推导，就可以得到上面的结论了
```

### Ending

ts中的Omit关键词就了解到这，希望大家学会之后，可以应用到我们的实际开发项目中。