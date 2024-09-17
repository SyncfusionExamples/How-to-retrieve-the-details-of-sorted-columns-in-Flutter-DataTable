# How to retrieve the details of sorted columns in Flutter DataTable (SfDataGrid)?.

In this article, we will show how to retrieve the details of sorted columns in [Flutter DataTable](https://www.syncfusion.com/flutter-widgets/flutter-datagrid).

Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) widget with the necessary properties. In the [DataGridSource](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridSource-class.html) class, override the [performSorting](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridSource/performSorting.html) method, which is triggered whenever sorting is performed. To track sorting information, maintain a list of [SortColumnDetails](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SortColumnDetails-class.html). If columns are sorted, update the sortColumnsDetails list with the current sorting information by using [DataGridSource.sortedColumns](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridSource/sortedColumns.html).

```dart
List<SortColumnDetails> sortColumnsDetails = <SortColumnDetails>[];

 @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Syncfusion Flutter DataGrid'),
      ),
      body: Column(
        children: [
          TextButton(
            onPressed: () {
              String sortedDetails = '';
              for (var details in sortColumnsDetails) {
                sortedDetails += 'Column Name: ${details.name}\n';
                sortedDetails += 'Sort Direction: ${details.sortDirection}\n\n';
              }

              showDialog(
                context: context,
                builder: (BuildContext context) {
                  return AlertDialog(
                    title: const Text('Sorted Details'),
                    content: Text(sortedDetails),
                    actions: [
                      TextButton(
                        onPressed: () {
                          Navigator.of(context).pop();
                        },
                        child: const Text('Close'),
                      ),
                    ],
                  );
                },
              );
            },
            child: const Text('Sorted Details'),
          ),
          Expanded(
            child: SfDataGrid(
              source: employeeDataSource,
              allowSorting: true,
              columnWidthMode: ColumnWidthMode.fill,
              columns: getColumn,
            ),
          ),
        ],
      ),
    );
  }

class EmployeeDataSource extends DataGridSource {
…..

  @override
  Future<void> performSorting(List<DataGridRow> rows) async {
    // save the sort column details.
    if (sortedColumns.isNotEmpty) {
      sortColumnsDetails.clear();
      sortColumnsDetails.addAll(sortedColumns);
    }
    // clear the sort column details when the sorting is cleared.
    else if (sortedColumns.isEmpty) {
      sortColumnsDetails.clear();
    }

    super.performSorting(rows);
  }
}
```

You can download this example on [GitHub](https://github.com/SyncfusionExamples/How-to-retrieve-the-details-of-sorted-columns-in-Flutter-DataTable).