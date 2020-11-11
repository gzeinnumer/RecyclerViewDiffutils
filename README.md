# RecyclerViewDiffutils
 https://www.youtube.com/watch?v=PAOcL1W05Eg

- `MyDiffUtilsCallBack.java`
```java
public class MyDiffUtilsCallBack extends DiffUtil.Callback {

    private List<String> oldList;
    private List<String> newList;

    public MyDiffUtilsCallBack(List<String> oldList, List<String> newList) {
        this.oldList = oldList;
        this.newList = newList;
    }

    @Override
    public int getOldListSize() {
        return oldList.size();
    }

    @Override
    public int getNewListSize() {
        return newList.size();
    }

    @Override
    public boolean areItemsTheSame(int oldItemPosition, int newItemPosition) {
        return oldItemPosition == newItemPosition;
    }

    @Override
    public boolean areContentsTheSame(int oldItemPosition, int newItemPosition) {
        return oldList.get(oldItemPosition) == newList.get(newItemPosition);
    }
}
```

- `MyAdapter.java`
```java
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.MyHolder> {

    private List<String> data;

    ...
    
    public void insertData(List<String> d){
        MyDiffUtilsCallBack diffUtilsCallBack = new MyDiffUtilsCallBack(d, data);
        DiffUtil.DiffResult diffResult = DiffUtil.calculateDiff(diffUtilsCallBack);

        data.addAll(d);
        diffResult.dispatchUpdatesTo(this);
    }

    public void updateData(List<String> d){
        MyDiffUtilsCallBack diffUtilsCallBack = new MyDiffUtilsCallBack(data, d);
        DiffUtil.DiffResult diffResult = DiffUtil.calculateDiff(diffUtilsCallBack);

        data.clear();
        data.addAll(d);
        diffResult.dispatchUpdatesTo(this);
    }
}
```

- `MainActivity.java`
```java
public class MainActivity extends AppCompatActivity {

    List<String> data = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnInsert.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                List<String> newList = new ArrayList<>();
                for (int i=0; i<3; i++)
                    newList.add(UUID.randomUUID().toString());
                    
                myAdapter.insertData(newList);
                rvData.smoothScrollToPosition(myAdapter.getItemCount() - 1);
            }
        });

        btnUpdate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                List<String> updateList = new ArrayList<>();
                for (int i=0; i<3; i++)
                    updateList.add(UUID.randomUUID().toString());
                    
                myAdapter.updateData(updateList);
            }
        });
    }

}
```

---

**FullCode [MainActivity](https://github.com/gzeinnumer/RecyclerViewDiffutils/blob/master/app/src/main/java/com/gzeinnumer/recyclerviewdiffutils/MainActivity.java) & [MyAdapter](https://github.com/gzeinnumer/RecyclerViewDiffutils/blob/master/app/src/main/java/com/gzeinnumer/recyclerviewdiffutils/MyAdapter.java) & [MyDiffUtilsCallBack](https://github.com/gzeinnumer/RecyclerViewDiffutils/blob/master/app/src/main/java/com/gzeinnumer/recyclerviewdiffutils/MyDiffUtilsCallBack.java)**

---

```
Copyright 2020 M. Fadli Zein
```
