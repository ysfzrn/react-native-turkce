# [FLATLIST](http://facebook.github.io/react-native/releases/0.49/docs/flatlist.html)

#### Pattern

```jsx
     <FlatList
        data={[]}
        ref={ref => {this.flatListRef = ref;}}
        renderItem={({ item }) => ( <Text>{item.name}</Text>)}
        ItemSeparatorComponent={this.renderSeparator}
        ListHeaderComponent={this.renderHeader}
        ListFooterComponent={this.renderFooter}
        keyExtractor={item => item.id}
        onEndReached={this.getMoreData}
        refreshing={this.state.refreshing}
        onRefresh={this.handleRefresh}
        onEndReachedThreshold={100}
      />
```

FlatList, performans açısından ve kullanım kolaylığı bakımından ListView'den daha iyi bir component. 

### Methodlar

#### **`scrollToEnd ({ })`**

#### **`scrollToIndex({ })`**

#### **`scrollToItem({ })`**

#### `scrollToOffset({ })`



