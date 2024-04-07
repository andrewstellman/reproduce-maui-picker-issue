# Reproduce a .NET MAUI Picker issue 

More details here: https://github.com/dotnet/maui/issues/21691

### Description

The Picker control seems to be broken on macOS. 
On macOS, clicking on the Picker displays a dialog that shows the title and a "Done" button but doesn't display the items:

<img width="305" alt="image" src="https://github.com/dotnet/maui/assets/7516297/f61b0f8d-fc6f-4c74-a0bd-38e18aaf4d86">

Here's a video that shows it in action:

https://github.com/dotnet/maui/assets/7516297/04f022b5-1aa5-4ca1-9c7b-f2d569f72cc6

I'm running it on the latest 8.0 GA SDK:

```
andrewstellman@Andrews-MBP reproduce-maui-picker-issue % dotnet workload list

Installed Workload Id      Manifest Version      Installation Source
--------------------------------------------------------------------
maui                       8.0.7/8.0.100         SDK 8.0.200        

Use `dotnet workload search` to find additional workloads to install.

andrewstellman@Andrews-MBP reproduce-maui-picker-issue % dotnet build -t:Run -f net8.0-maccatalyst

Welcome to .NET 8.0!
---------------------
SDK Version: 8.0.203
```

This same code works fine on Windows and Android:

Windows:
<img width="1352" alt="image" src="https://github.com/dotnet/maui/assets/7516297/75149e96-9407-4adf-b528-82672b34e43e">

Android:
<img width="500" alt="image" src="https://github.com/dotnet/maui/assets/7516297/c40586ff-bcf1-47e0-af20-65ee1c2f92c8">



### Steps to Reproduce

Here's some simple code to reproduce the issue:

XAML
```xaml
            <Picker x:Name="BirdPicker" Title="Pick a bird" />

            <Button x:Name="AddBird" Text="Add a bird"
                Margin="0,0,0,20" SemanticProperties.Hint="Adds a bird"
                Clicked="AddBird_Clicked"/>

            <Label x:Name="Birds"
                    Padding="10" MinimumWidthRequest="150"
                    TextColor="White" BackgroundColor="DarkBlue" 
                    SemanticProperties.Description="Shows the added birds" />
```

C#
```C#
	public MainPage()
	{
		InitializeComponent();

		BirdPicker.ItemsSource = new List<string> {
			"Duck",
			"Pigeon",
			"Penguin",
			"Ostrich",
			"Owl"
		};
	}

	private void AddBird_Clicked(object sender, EventArgs e)
	{
		if (!String.IsNullOrEmpty(Birds.Text))
		{
			Birds.Text = Birds.Text + Environment.NewLine;
		}
		Birds.Text += BirdPicker.SelectedItem;
	}
```

