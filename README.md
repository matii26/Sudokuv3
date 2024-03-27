<Window x:Class="Sudoku.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Sudoku" Height="450" Width="450">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <Grid x:Name="sudokuGrid" Margin="10">
            <Grid.Resources>
                <Style TargetType="TextBox">
                    <Setter Property="FontSize" Value="20" />
                    <Setter Property="TextAlignment" Value="Center" />
                    <Setter Property="MaxLength" Value="1" />
                    <Setter Property="Width" Value="50" />
                    <Setter Property="Height" Value="50" />
                    <Setter Property="IsReadOnly" Value="True" />
                    <Setter Property="BorderThickness" Value="1" />
                    <Setter Property="BorderBrush" Value="Black" />
                    <Setter Property="Padding" Value="0" />
                    <Style.Triggers>
                        <Trigger Property="Text" Value="">
                            <Setter Property="Background" Value="White" />
                        </Trigger>
                        <Trigger Property="IsReadOnly" Value="True">
                            <Setter Property="Background" Value="LightGray" />
                        </Trigger>
                    </Style.Triggers>
                </Style>
            </Grid.Resources>

            <!-- Define 4x4 Sudoku grid -->
            <TextBox Grid.Row="0" Grid.Column="0" x:Name="txt00"/>
            <TextBox Grid.Row="0" Grid.Column="1" x:Name="txt01"/>
            <TextBox Grid.Row="0" Grid.Column="2" x:Name="txt02"/>
            <TextBox Grid.Row="0" Grid.Column="3" x:Name="txt03"/>
            
            <TextBox Grid.Row="1" Grid.Column="0" x:Name="txt10"/>
            <TextBox Grid.Row="1" Grid.Column="1" x:Name="txt11"/>
            <TextBox Grid.Row="1" Grid.Column="2" x:Name="txt12"/>
            <TextBox Grid.Row="1" Grid.Column="3" x:Name="txt13"/>

            <TextBox Grid.Row="2" Grid.Column="0" x:Name="txt20"/>
            <TextBox Grid.Row="2" Grid.Column="1" x:Name="txt21"/>
            <TextBox Grid.Row="2" Grid.Column="2" x:Name="txt22"/>
            <TextBox Grid.Row="2" Grid.Column="3" x:Name="txt23"/>
            
            <TextBox Grid.Row="3" Grid.Column="0" x:Name="txt30"/>
            <TextBox Grid.Row="3" Grid.Column="1" x:Name="txt31"/>
            <TextBox Grid.Row="3" Grid.Column="2" x:Name="txt32"/>
            <TextBox Grid.Row="3" Grid.Column="3" x:Name="txt33"/>
        </Grid>

        <Button Grid.Row="1" Grid.Column="1" Content="Check" Click="Check_Click" />
    </Grid>
</Window>


using System;
using System.Windows;
using System.Windows.Controls;

namespace Sudoku
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            InitializeSudoku();
        }

        private void InitializeSudoku()
        {
            // Inicjalizacja wartości pól planszy Sudoku
            txt00.Text = "";
            txt01.Text = "";
            txt02.Text = "3";
            txt03.Text = "";

            txt10.Text = "";
            txt11.Text = "";
            txt12.Text = "";
            txt13.Text = "";

            txt20.Text = "2";
            txt21.Text = "";
            txt22.Text = "";
            txt23.Text = "1";

            txt30.Text = "";
            txt31.Text = "1";
            txt32.Text = "";
            txt33.Text = "";
        }

        private void Check_Click(object sender, RoutedEventArgs e)
        {
            if (IsSudokuValid())
            {
                MessageBox.Show("Sudoku puzzle is valid!");
            }
            else
            {
                MessageBox.Show("Sudoku puzzle is invalid!");
            }
        }

        private bool IsSudokuValid()
        {
            // Sprawdzenie wierszy i kolumn
            for (int i = 0; i < 4; i++)
            {
                if (!IsRowValid(i) || !IsColumnValid(i))
                {
                    return false;
                }
            }

            // Sprawdzenie kwadratów 2x2
            for (int i = 0; i < 4; i += 2)
            {
                for (int j = 0; j < 4; j += 2)
                {
                    if (!IsSquareValid(i, j))
                    {
                        return false;
                    }
                }
            }

            return true;
        }

        private bool IsRowValid(int row)
        {
            for (int i = 0; i < 4; i++)
            {
                for (int j = i + 1; j < 4; j++)
                {
                    TextBox currentBox = (TextBox)FindName("txt" + row + i);
                    TextBox compareBox = (TextBox)FindName("txt" + row + j);
                    if (currentBox.Text != "" && currentBox.Text == compareBox.Text)
                    {
                        return false;
                    }
                }
            }
            return true;
        }

        private bool IsColumnValid(int column)
        {
            for (int i = 0; i < 4; i++)
            {
                for (int j = i + 1; j < 4; j++)
                {
                    TextBox currentBox = (TextBox)FindName("txt" + i + column);
                    TextBox compareBox = (TextBox)FindName("txt" + j + column);
                    if (currentBox.Text != "" && currentBox.Text == compareBox.Text)
                    {
                        return false;
                    }
                }
            }
            return true;
        }

        private bool IsSquareValid(int row, int column)
        {
            for (int i = row; i < row + 2; i++)
            {
                for (int j = column; j < column + 2; j++)
                {
                    for (int k = row; k < row + 2; k++)
                    {
                        for (int l = column; l < column + 2; l++)
                        {
                            if (i != k || j != l)
                            {
                                TextBox currentBox = (TextBox)FindName("txt" + i + j);
                                TextBox compareBox = (TextBox)FindName("txt" + k + l);
                                if (currentBox.Text != "" && currentBox.Text == compareBox.Text)
                                {
                                    return false;
                                }
                            }
                        }
                    }
                }
            }
            return true;
        }
    }
}
