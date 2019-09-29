# Enums and @IntDefs

Enumerations serve the purpose of representing a group of named constants in a programming language  
We often use `Enums` to make our cool, restricted group of values tht a function or statement could take.  
But they are usually considered a bad practise in android because Enums often require more than twice as much memory as static constants.

## Enter `IntDef`  
`@IntDef` is a way of replacing an integer enum where there's a parameter that should only
 accept explicit int values.We can use IntDef to ensure that the value is one of the expected values
 by adding this annotation. For eg:  
``` java

public class example {
   @IntDef( {Type.TYPE_MUSIC,Type.TYPE_PHOTO,Type.TYPE_TEXT})
    public  @interface  Type{
        int TYPE_MUSIC = 0;
        int TYPE_PHOTO = 21;
        int TYPE_TEXT = 42;
    }

  // Mark the argument as restricted to these enumerated types
  public void getItemType(@Type int itemType) {
    int res = itemType;
  }
}
```  

The function will allow user to pass any integer value, but will give a lint warning if value is not
`Type.TYPE_MUSIC`,`Type.TYPE_PHOTO`or`Type.TYPE_TEXT`(yes, even if 0,21 or 42 are passed directly 
too ).