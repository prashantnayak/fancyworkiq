# FancyWorkIQ

A Blazor Web App starter project for the Techconnect WorkIQ hackathon.

## What is WorkIQ?

WorkIQ (or FancyWorkIQ) is a modern web application framework built on **Blazor Server** and **.NET 10** that provides a foundation for building interactive, real-time web applications. It's designed as a starter template for the TechConnect hackathon, offering a clean architecture and best practices for rapid application development.

## Overview

This is a Blazor Web App built with .NET 10, featuring:
- **Interactive server-side rendering with Blazor Server** - Real-time UI updates without page refreshes
- **Responsive UI with Bootstrap 5** - Mobile-friendly, professional design
- **Component-based architecture** - Reusable, maintainable code structure
- **Sample pages: Home, Counter, and Weather** - Working examples to learn from

## Prerequisites

- [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0) or later

## Getting Started

### Build the project

```bash
dotnet build
```

### Run the application

```bash
dotnet run
```

The application will start and be available at `https://localhost:7060` (or `http://localhost:5090`).

### Development

To run in watch mode for development:

```bash
dotnet watch run
```

This will automatically rebuild and restart the application when you make changes to the code.

## Project Structure

- `Components/` - Blazor components and pages
  - `Pages/` - Page components (Home, Counter, Weather)
  - `Layout/` - Layout components (MainLayout, NavMenu)
- `wwwroot/` - Static files (CSS, JavaScript, images)
- `Program.cs` - Application entry point and configuration
- `FancyWorkIQ.csproj` - Project file

## Features

- **Home Page**: Welcome page with basic information
- **Counter Page**: Interactive counter demonstrating Blazor's reactivity
- **Weather Page**: Sample data display showing how to work with data in Blazor

## Learn More

- [Blazor Documentation](https://learn.microsoft.com/aspnet/core/blazor/)
- [.NET Documentation](https://learn.microsoft.com/dotnet/)

## How to Use WorkIQ in Your Project

### 1. Clone and Setup

```bash
# Clone this repository
git clone https://github.com/prashantnayak/fancyworkiq.git
cd fancyworkiq

# Restore dependencies
dotnet restore

# Build the project
dotnet build
```

### 2. Understanding the Architecture

WorkIQ follows a clean Blazor component architecture:

**Key Concepts:**
- **Components**: Reusable UI elements (`.razor` files) that combine HTML and C#
- **Pages**: Special components with `@page` directive that define routes
- **Layouts**: Wrapper components that define the structure of your pages
- **Services**: Shared business logic that can be injected into components

### 3. Creating Your First Component

Create a new component in `Components/Pages/MyPage.razor`:

```razor
@page "/mypage"

<PageTitle>My Page</PageTitle>

<h1>My Custom Page</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### 4. Adding Navigation

Add your page to the navigation menu in `Components/Layout/NavMenu.razor`:

```razor
<div class="nav-item px-3">
    <NavLink class="nav-link" href="mypage">
        <span class="bi bi-list-nested-nav-menu" aria-hidden="true"></span> My Page
    </NavLink>
</div>
```

### 5. Working with Data

Create a service in a new file `Services/DataService.cs`:

```csharp
namespace FancyWorkIQ.Services;

public class DataService
{
    public Task<List<string>> GetDataAsync()
    {
        return Task.FromResult(new List<string> 
        { 
            "Item 1", 
            "Item 2", 
            "Item 3" 
        });
    }
}
```

Register it in `Program.cs`:

```csharp
builder.Services.AddScoped<DataService>();
```

Use it in your component:

```razor
@inject DataService DataService

@code {
    private List<string> items = new();

    protected override async Task OnInitializedAsync()
    {
        items = await DataService.GetDataAsync();
    }
}
```

### 6. Styling Your Application

- Modify global styles in `wwwroot/css/app.css`
- Add component-specific styles using `<style>` tags in `.razor` files
- Use Bootstrap 5 classes for responsive design

### 7. Common Patterns

**Form Handling:**
```razor
<EditForm Model="@model" OnValidSubmit="HandleSubmit">
    <InputText @bind-Value="model.Name" />
    <button type="submit">Submit</button>
</EditForm>
```

**API Calls:**
```csharp
@inject HttpClient Http

@code {
    private async Task<WeatherData> FetchWeatherAsync()
    {
        return await Http.GetFromJsonAsync<WeatherData>("api/weather");
    }
}
```

**State Management:**
```csharp
// Use cascading values for shared state across components
<CascadingValue Value="@sharedState">
    @Body
</CascadingValue>
```

### 8. Building for Production

```bash
# Build optimized version
dotnet publish -c Release -o ./publish

# Run the published app
cd publish
dotnet FancyWorkIQ.dll
```

### 9. Best Practices

1. **Keep components small and focused** - Each component should have a single responsibility
2. **Use dependency injection** - Register services and inject them into components
3. **Async/await for data operations** - Always use async methods for I/O operations
4. **Error handling** - Use try-catch blocks and display user-friendly error messages
5. **Validation** - Use data annotations and EditForm for form validation
6. **Security** - Never expose sensitive data to the client, validate all inputs

### 10. Troubleshooting

**Port already in use:**
```bash
dotnet run --urls "http://localhost:5000"
```

**Hot reload not working:**
```bash
dotnet watch run --non-interactive
```

**Connection lost errors:**
- Check browser console for JavaScript errors
- Verify WebSocket connections are not blocked by firewall
- Ensure app is running on HTTPS in production

## Next Steps

- Explore the sample pages (Counter, Weather) to understand Blazor patterns
- Read the [official Blazor tutorial](https://learn.microsoft.com/aspnet/core/blazor/tutorials/)
- Check out [Blazor component libraries](https://github.com/AdrienTorris/awesome-blazor) for pre-built UI components
- Join the Blazor community on [Discord](https://aka.ms/blazor-discord) or [GitHub](https://github.com/dotnet/aspnetcore)
