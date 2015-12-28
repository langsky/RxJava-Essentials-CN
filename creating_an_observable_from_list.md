# 从列表创建一个Observable

在这个例子中，我们将引入`from()`函数。使用这个特殊的“创建”函数，我们可以从一个列表中创建一个Observable。Observable将发射出列表中的每一个元素，我们可以对这些发出的元素订阅对应的响应。

为了实现和第一个例子同样的结果，我们在每一个`onNext()`函数更新我们的适配器，添加元素并通知插入。

我们将复用和第一个例子同样的结构。主要的不同的是我们不再检索已安装的应用列表。列表由外部实体提供：

```java
mApps = ApplicationsList.getInstance().getList();
```
获得列表后，我们仅需将它响应化并填充RecyclerView的item:
```java
private void loadList(List<AppInfo> apps) {
    mRecyclerView.setVisibility(View.VISIBLE);
    Observable.from(apps)
            .subscribe(new Observable<AppInfo>() {

                @Override
                public void onCompleted() {
                    mSwipeRefreshLayout.setRefreshing(false);
                    Toast.makeText(getActivity(), "Here is the list!", Toast.LENGTH_LONG).show();
                }

                @Override
                public void onError(Throwable e) {
                    Toast.makeText(getActivity(), "Something went wrong!", Toast.LENGTH_SHORT).show();
                    mSwipeRefreshLayout.setRefreshing(false);
                }

                @Override
                public void onNext(AppInfo appInfo) {
                    mAddedApps.add(appInfo); 
                    mAdapter.addApplications(appInfos);
                    mSwipeRefreshLayout.setRefreshing(false);
                }
            });
}
```
