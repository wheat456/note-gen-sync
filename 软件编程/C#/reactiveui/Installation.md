ReactiveUI.WPF
ReactiveUI.SourceGenerators
`<TargetFramework>net10.0-windows10.0.19041.0</TargetFramework>`

```cs
protected override void OnStartup(StartupEventArgs e)
    {
        base.OnStartup(e);

        // Initialize ReactiveUI with RxAppBuilder
        var app = RxAppBuilder.CreateReactiveUIBuilder()
            .WithWpf()
            .WithViewsFromAssembly(Assembly.GetExecutingAssembly())
            .WithRegistration(locator =>
            {
                // Register your services here
                locator.RegisterLazySingleton<IScreen>(() => new MainViewModel());
                locator.RegisterLazySingleton<INavigationService>(() => new NavigationService());
                locator.RegisterLazySingleton<IDataService>(() => new DataService());
            })
            .BuildApp();

        // Optional: Store schedulers for later use
        // var mainScheduler = app.MainThreadScheduler;
        // var taskScheduler = app.TaskpoolScheduler;

        // Create and show main window
        var mainWindow = new MainWindow();
        mainWindow.Show();
    }
```