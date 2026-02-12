# WorkIQ Usage Guide

This guide provides comprehensive instructions on how to use FancyWorkIQ (WorkIQ) in your projects.

## Table of Contents

1. [Introduction](#introduction)
2. [Installation & Setup](#installation--setup)
3. [Project Structure](#project-structure)
4. [Creating Components](#creating-components)
5. [Working with Data](#working-with-data)
6. [Routing & Navigation](#routing--navigation)
7. [Styling](#styling)
8. [Forms & Validation](#forms--validation)
9. [Deployment](#deployment)
10. [Best Practices](#best-practices)

## Introduction

WorkIQ is a Blazor Server application template that enables you to build interactive web applications using C# instead of JavaScript. It leverages the power of .NET 10 and provides real-time UI updates through SignalR connections.

### Why Use WorkIQ?

- **Full-Stack C#**: Write both client and server code in C#
- **Real-time Updates**: Automatic UI updates without page refreshes
- **Strong Typing**: Catch errors at compile time
- **Rich Ecosystem**: Access to the entire .NET ecosystem
- **SEO Friendly**: Server-side rendering improves SEO
- **Code Reusability**: Share code between client and server

## Installation & Setup

### Prerequisites

- .NET 10 SDK or later
- Visual Studio 2022, VS Code, or any text editor
- Basic knowledge of C# and HTML

### Initial Setup

```bash
# Clone the repository
git clone https://github.com/prashantnayak/fancyworkiq.git
cd fancyworkiq

# Restore dependencies
dotnet restore

# Build the project
dotnet build

# Run the application
dotnet run
```

The application will be available at:
- HTTPS: `https://localhost:7060`
- HTTP: `http://localhost:5090`

### Development Mode

For development with hot reload:

```bash
dotnet watch run
```

This automatically rebuilds and restarts the app when you make changes.

## Project Structure

```
FancyWorkIQ/
├── Components/              # All Blazor components
│   ├── Layout/             # Layout components
│   │   ├── MainLayout.razor       # Main layout template
│   │   ├── NavMenu.razor          # Navigation menu
│   │   └── ReconnectModal.razor   # Connection recovery UI
│   ├── Pages/              # Page components (routes)
│   │   ├── Home.razor             # Home page
│   │   ├── Counter.razor          # Counter demo
│   │   ├── Weather.razor          # Weather data demo
│   │   ├── Error.razor            # Error page
│   │   └── NotFound.razor         # 404 page
│   ├── App.razor           # Root component
│   └── _Imports.razor      # Shared imports
├── wwwroot/                # Static files
│   ├── css/                # Stylesheets
│   ├── js/                 # JavaScript files
│   └── bootstrap/          # Bootstrap assets
├── Properties/             # Project properties
│   └── launchSettings.json # Development settings
├── Program.cs              # Application entry point
├── appsettings.json        # Configuration
└── FancyWorkIQ.csproj     # Project file
```

## Creating Components

### Basic Component

Create a new file `Components/Pages/MyComponent.razor`:

```razor
@page "/mycomponent"

<PageTitle>My Component</PageTitle>

<h3>My Component</h3>

<p>Hello from my component!</p>

@code {
    // C# code goes here
}
```

### Component with Parameters

```razor
<GreetingComponent Name="John" Age="25" />

@code {
    // In GreetingComponent.razor
}
```

```razor
<h3>Hello, @Name!</h3>
<p>You are @Age years old.</p>

@code {
    [Parameter]
    public string Name { get; set; } = string.Empty;
    
    [Parameter]
    public int Age { get; set; }
}
```

### Component Lifecycle

```razor
@code {
    protected override void OnInitialized()
    {
        // Called when component is initialized
        Console.WriteLine("Component initialized");
    }
    
    protected override async Task OnInitializedAsync()
    {
        // Async initialization
        await LoadDataAsync();
    }
    
    protected override void OnParametersSet()
    {
        // Called when parameters are set
    }
    
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Called after first render
            await JSRuntime.InvokeVoidAsync("initializeComponent");
        }
    }
}
```

## Working with Data

### Creating a Service

Create `Services/TodoService.cs`:

```csharp
namespace FancyWorkIQ.Services;

public class TodoService
{
    private readonly List<TodoItem> _todos = new();
    
    public Task<List<TodoItem>> GetTodosAsync()
    {
        return Task.FromResult(_todos.ToList());
    }
    
    public Task AddTodoAsync(TodoItem item)
    {
        _todos.Add(item);
        return Task.CompletedTask;
    }
    
    public Task DeleteTodoAsync(int id)
    {
        var item = _todos.FirstOrDefault(t => t.Id == id);
        if (item != null)
        {
            _todos.Remove(item);
        }
        return Task.CompletedTask;
    }
}

public class TodoItem
{
    public int Id { get; set; }
    public string Title { get; set; } = string.Empty;
    public bool IsCompleted { get; set; }
}
```

### Registering the Service

In `Program.cs`:

```csharp
builder.Services.AddScoped<TodoService>();
```

### Using the Service in a Component

```razor
@page "/todos"
@inject TodoService TodoService

<h3>My Todos</h3>

@foreach (var todo in todos)
{
    <div>
        <input type="checkbox" @bind="todo.IsCompleted" />
        <span>@todo.Title</span>
    </div>
}

@code {
    private List<TodoItem> todos = new();
    
    protected override async Task OnInitializedAsync()
    {
        todos = await TodoService.GetTodosAsync();
    }
}
```

### HTTP Calls

```razor
@inject HttpClient Http

@code {
    private async Task<WeatherForecast[]> FetchWeatherAsync()
    {
        return await Http.GetFromJsonAsync<WeatherForecast[]>(
            "https://api.example.com/weather");
    }
}
```

## Routing & Navigation

### Defining Routes

```razor
@page "/products"              <!-- Single route -->
@page "/products/{id:int}"     <!-- Route with parameter -->
@page "/items/{category}"      <!-- Route with string parameter -->
```

### Navigation

```razor
@inject NavigationManager Navigation

<button @onclick="GoToCounter">Go to Counter</button>

@code {
    private void GoToCounter()
    {
        Navigation.NavigateTo("/counter");
    }
}
```

### Route Parameters

```razor
@page "/product/{id:int}"

<h3>Product @Id</h3>

@code {
    [Parameter]
    public int Id { get; set; }
    
    protected override void OnParametersSet()
    {
        // Load product with Id
    }
}
```

### Query Parameters

```razor
@inject NavigationManager Navigation

@code {
    protected override void OnInitialized()
    {
        var uri = Navigation.ToAbsoluteUri(Navigation.Uri);
        if (QueryHelpers.ParseQuery(uri.Query).TryGetValue("id", out var id))
        {
            // Use the id parameter
        }
    }
}
```

## Styling

### Global Styles

Modify `wwwroot/css/app.css`:

```css
.my-custom-style {
    color: blue;
    font-weight: bold;
}
```

### Component-Specific Styles

In your `.razor` file:

```razor
<div class="my-component">
    <h3>Styled Component</h3>
</div>

<style>
    .my-component {
        background-color: #f0f0f0;
        padding: 20px;
        border-radius: 8px;
    }
</style>
```

### Using Bootstrap

WorkIQ includes Bootstrap 5:

```razor
<button class="btn btn-primary">Primary Button</button>
<div class="alert alert-success">Success message!</div>
<div class="card">
    <div class="card-body">Card content</div>
</div>
```

### Dynamic CSS Classes

```razor
<div class="@GetCssClass()">
    Content
</div>

@code {
    private bool isActive = true;
    
    private string GetCssClass() => isActive ? "active" : "inactive";
}
```

## Forms & Validation

### Basic Form

```razor
<EditForm Model="@person" OnValidSubmit="HandleSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />
    
    <div class="mb-3">
        <label>Name:</label>
        <InputText @bind-Value="person.Name" class="form-control" />
        <ValidationMessage For="@(() => person.Name)" />
    </div>
    
    <div class="mb-3">
        <label>Email:</label>
        <InputText @bind-Value="person.Email" class="form-control" />
        <ValidationMessage For="@(() => person.Email)" />
    </div>
    
    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    private Person person = new();
    
    private void HandleSubmit()
    {
        // Form is valid, process the data
        Console.WriteLine($"Submitted: {person.Name}");
    }
}
```

### Model with Validation

```csharp
using System.ComponentModel.DataAnnotations;

public class Person
{
    [Required(ErrorMessage = "Name is required")]
    [StringLength(50, MinimumLength = 2)]
    public string Name { get; set; } = string.Empty;
    
    [Required]
    [EmailAddress]
    public string Email { get; set; } = string.Empty;
    
    [Range(18, 100, ErrorMessage = "Age must be between 18 and 100")]
    public int Age { get; set; }
}
```

## Deployment

### Publishing the Application

```bash
# Publish for production
dotnet publish -c Release -o ./publish

# The output will be in the ./publish directory
```

### Running in Production

```bash
cd publish
dotnet FancyWorkIQ.dll --urls "http://0.0.0.0:80"
```

### Docker Deployment

Create a `Dockerfile`:

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src
COPY ["FancyWorkIQ.csproj", "./"]
RUN dotnet restore
COPY . .
RUN dotnet build -c Release -o /app/build

FROM build AS publish
RUN dotnet publish -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FancyWorkIQ.dll"]
```

Build and run:

```bash
docker build -t fancyworkiq .
docker run -d -p 8080:80 fancyworkiq
```

## Best Practices

### 1. Component Organization

- Keep components small and focused (Single Responsibility Principle)
- Create reusable components in `Components/Shared/`
- Use partial classes for complex component logic

### 2. State Management

```razor
@code {
    // ✅ Good: Component state
    private int counter = 0;
    
    // ✅ Good: Use services for shared state
    [Inject] private AppStateService AppState { get; set; }
}
```

### 3. Performance

- Use `@key` directive for list items
- Implement `ShouldRender()` for optimization
- Use `StateHasChanged()` wisely

```razor
@foreach (var item in items)
{
    <div @key="item.Id">@item.Name</div>
}

@code {
    protected override bool ShouldRender()
    {
        // Only render when necessary
        return shouldUpdate;
    }
}
```

### 4. Error Handling

```razor
@code {
    private string errorMessage = string.Empty;
    
    private async Task LoadDataAsync()
    {
        try
        {
            data = await DataService.GetDataAsync();
        }
        catch (Exception ex)
        {
            errorMessage = "Failed to load data. Please try again.";
            Console.WriteLine($"Error: {ex.Message}");
        }
    }
}
```

### 5. Security

- Never expose sensitive data to the client
- Validate all user inputs
- Use authentication and authorization
- Sanitize HTML content

```razor
@* ❌ Bad: Unsafe HTML *@
@((MarkupString)userInput)

@* ✅ Good: Escaped by default *@
@userInput
```

### 6. Async Operations

```razor
@code {
    // ✅ Good: Async/await for I/O
    protected override async Task OnInitializedAsync()
    {
        data = await Http.GetFromJsonAsync<Data>("api/data");
    }
    
    // ❌ Bad: Blocking call
    protected override void OnInitialized()
    {
        data = Http.GetFromJsonAsync<Data>("api/data").Result;
    }
}
```

## Common Issues & Solutions

### Issue: Connection Lost

**Solution**: The ReconnectModal component handles this automatically. Ensure WebSockets are not blocked.

### Issue: Hot Reload Not Working

**Solution**: 
```bash
dotnet watch run --non-interactive
```

### Issue: Port Already in Use

**Solution**:
```bash
dotnet run --urls "http://localhost:5000"
```

### Issue: Changes Not Reflecting

**Solution**: Hard refresh the browser (Ctrl+Shift+R) or clear browser cache.

## Additional Resources

- [Official Blazor Documentation](https://learn.microsoft.com/aspnet/core/blazor/)
- [Blazor University](https://blazor-university.com/)
- [Awesome Blazor](https://github.com/AdrienTorris/awesome-blazor)
- [Blazor Component Libraries](https://github.com/AdrienTorris/awesome-blazor#libraries--extensions)

## Support

For issues or questions:
- Create an issue on [GitHub](https://github.com/prashantnayak/fancyworkiq/issues)
- Check the [Blazor documentation](https://learn.microsoft.com/aspnet/core/blazor/)
- Join the [Blazor Discord community](https://aka.ms/blazor-discord)

---

Happy coding with WorkIQ! 🚀
