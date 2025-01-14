### Tree

<!--start-code-->

```js
const data = mockTreeData({
  limits: [2, 3, 3],
  labels: layer => {
    if (layer === 0) {
      return faker.vehicle.manufacturer();
    } else if (layer === 1) {
      return faker.vehicle.fuel();
    }
    return faker.vehicle.vehicle();
  },
  getRowData: () => ({
    price: faker.commerce.price(10000, 1000000, 0, '$', true),
    rating: faker.finance.amount(2, 5)
  })
});

const App = () => {
  const [tree, setTree] = React.useState(true);
  return (
    <div>
      <label>
        <input
          type="checkbox"
          checked={tree}
          onChange={() => {
            setTree(!tree);
          }}
        />
        isTree
      </label>
      <Table
        virtualized
        isTree={tree}
        defaultExpandAllRows
        bordered
        cellBordered
        rowKey="value"
        height={400}
        data={data}
        /** shouldUpdateScroll: whether to update the scroll bar after data update **/
        shouldUpdateScroll={false}
        onExpandChange={(isOpen, rowData) => {
          console.log(isOpen, rowData);
        }}
        renderTreeToggle={(icon, rowData) => {
          if (rowData.children && rowData.children.length === 0) {
            return <SpinnerIcon spin />;
          }
          return icon;
        }}
      >
        <Column flexGrow={1}>
          <HeaderCell>Vehicle 🚗</HeaderCell>
          <Cell dataKey="label" />
        </Column>
        <Column width={180}>
          <HeaderCell>Rating ⭐️</HeaderCell>
          <Cell>
            {rowData =>
              Array.from({ length: rowData.rating }).map((_, i) => <span key={i}>⭐️</span>)
            }
          </Cell>
        </Column>
        <Column width={100}>
          <HeaderCell>Price 💰</HeaderCell>
          <Cell dataKey="price" />
        </Column>
      </Table>
    </div>
  );
};

ReactDOM.render(<App />);
```

<!--end-code-->
