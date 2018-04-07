To resize array size, we have to take a way like creating a function and then recall it that will dynamically resize the size of an already-created array.
*Example code as below to implement*
```java
    // assmue that the default array size is 10;
    int[] oldArray = new int[10];
    // create a function to implement to dynamically resize the oldArray
    public void resizArray(int newSize){
        int[] newArray = new int[newSize];
        for(int i = 0; i < oldArray.length; i++){
            newArray[i] = oldArray[i];
        }
        oldArray = newArray;
    }
    // implementation
    
    public void add(int number){
        if(oldArray.length <= 10){
            resizeArray(oldArray.length * 2);
        }
        .....
        .....
    }

