# Map_UpdateKey_Java
JavaのMapのキーを変更しても機能するか

## ソース

``` App.java
package ittimfn.sample;

import java.util.HashMap;
import java.util.Map;

import ittimfn.sample.model.Model;

/**
 * キーを後で変えても機能するか？
 *
 */
public class App {
    public static void main( String[] args ) {
        Map<Model, String> map = new HashMap<Model, String>();
        
        map.put(Model.builder().key(1).tmp(10).build(), "hoge");
        map.put(Model.builder().key(2).tmp(11).build(), "piyo");
        
        System.out.println(map);
        // 取得できる。
        System.out.println(map.get(Model.builder().key(1).tmp(10).build()));
        
        int key = 3;
        for(Model model : map.keySet()) {
            model.setKey(key++);
        }
        
        // キーが持っている値が変わっていることは見える。
        System.out.println(map);
        // 取得できない。
        System.out.println(map.get(Model.builder().key(3).tmp(10).build()));
        // 取得できない。
        System.out.println(map.get(Model.builder().key(1).tmp(10).build()));
    }
}

```

``` Model.java
package ittimfn.sample.model;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.EqualsAndHashCode;

@Data
@AllArgsConstructor
@Builder
@EqualsAndHashCode(onlyExplicitlyIncluded = true)
public class Model {
    
    @EqualsAndHashCode.Include
    private int key;
    
    private int tmp;
}

```

## 実行結果

``` txt
{Model(key=1, tmp=10)=hoge, Model(key=2, tmp=11)=piyo}
hoge
{Model(key=3, tmp=10)=hoge, Model(key=4, tmp=11)=piyo}
null
null
```

## 実行

``` bash
mvn clean compile exec:java
```