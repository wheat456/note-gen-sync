先定义一个
```cs
public Interaction<string, bool> ConfirmDelete { get; }=new();
```
然后再命令中，调用handle
```cs
	这里就是返回值=ConfirmDelete.Handle(这里传入上面定义的类型比如string)
```
在view中定义handle处理函数
```cs
this.ViewModel.ConfirmDelete.RegisterHandler(interaction =>
            {
                // 这里编写具体的 UI 逻辑
                var result = MessageBox.Show(
                    interaction.Input, 
                    "提示", 
                    MessageBoxButton.YesNo);

                // 将结果返回给 ViewModel
                interaction.SetOutput(result == MessageBoxResult.Yes);
            })
            .DisposeWith(disposables);
```
