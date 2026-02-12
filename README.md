# FancyWorkIQ

A Blazor Web App starter project for the Techconnect workiq hackathon.

## Overview

This is a Blazor Web App built with .NET 10, featuring:
- Interactive server-side rendering with Blazor Server
- Responsive UI with Bootstrap 5
- Sample pages: Home, Counter, and Weather

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

The application will start and be available at `https://localhost:5001` (or `http://localhost:5000`).

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
