Are you tired of "C#" in-game scripting in Space Engineers?
Well, there we are.
Our project will allow you to write in-game scripts in Python and export it into "C#".
For example, there is Python code:
no a v pythonu by to mohlo finálně vypadat nějak takhle:
```py
UpdateFrequency(100)
lcd = GetBlockByName.TextPanel("LcdStatus")
batteries = GetBlockByGroup.Battery("Batteries")
cargos = GetBlockByGroup.Battery("Cargos")

checkIfExist([lcd, batteries, cargos])
batteryPercent = getBatteryPercentage(batteries)

if batteryPercent > 80:
  textColor = "green"
elif batteryPercent <= 80 && batteryPercent > 40:
  textColor = "orange"
else:
  textColor = "red"

BatteryLevelText = ColorText(textColor, f"{batteryPercent} %")
batteriesInput = GetInput.Battery(batteries)
batteriesOutput = GetOutput.Battery(batteries)

if batteriesInput > batteriesOutput:
  InOutText = ColorText("green", batteriesInput)
else:
  InOutText = ColorText("red", batteriesOutput)

loadingBar = createLoadingBar(batteryPercent)
LcdText = f"{loadingBar} {BatteryLevelText}\n{InOutText}"
DisplayText(lcd, LcdText)
```
And you will be able to export it into this ugly "C#" code:
```cs
public Program()
{
    Runtime.UpdateFrequency = UpdateFrequency.Update100;
}

public void Main(string argument, UpdateType updateSource)
{
    // Definice bloků
    var lcd = GridTerminalSystem.GetBlockWithName("LcdStatus") as IMyTextPanel;
    var batteries = GridTerminalSystem.GetBlockGroupWithName("Batteries") as IMyBatteryBlock;
    var cargos = GridTerminalSystem.GetBlockGroupWithName("Cargos") as IMyCargoContainer;

    // Kontrola existence
    if (lcd == null || batteries == null || lights == null || cargos == null)
    {
        Echo("Některý z bloků nebyl nalezen. Zkontrolujte názvy.");
        return;
    }

    // Výpočet procent baterie, určení barvy úrovně, zjištění inputu / outputu (vypisuje pouze to, co je větší)
    float batteryLevel = batteries.CurrentStoredPower / batteries.MaxStoredPower * 100;

    string BatteryTextColor = "white";
    if (batteryLevel > 80)
    {
        BatteryTextColor = "green";
    }
    else if (batteryLevel <= 80 && batteryLevel > 40)
    {
        BatteryTextColor = "orange";
    }
    else
    {
        BatteryTextColor = "red";
    }

    string batteryStatus = $"Baterie: <color={BatteryTextColor}>{batteryLevel:0.00}%</color>\n";

    float input = batteries.CurrentInput;
    float output = batteries.CurrentOutput;
    string powerFlow = input >= output ? $"<color=green>Input: {input:0.00} Kwh</color>" : $"<color=red>Output: {output:0.00} Kwh</color>\n";

    string divider = " \n"

    int roundedBatteryLevel = (int)Math.Round(batteryLevel / 10.0) * 10;
    int numberOfEquals = roundedBatteryLevel / 10;

    // Vytvoření loading baru
    string batteryPercent = "[";
    for (int i = 0; i < numberOfEquals; i++)
    {
        batteryPercent += "=";
    }
    for (int i = numberOfEquals; i < 10; i++)
    {
        batteryPercent += "-";
    }
    batteryPercent += "]";  

    lcd.WriteText(batteryPercent + batteryStatus + powerFlow + divider, false);
}
```
Are you interested? Just wait or contact us to colaborate!
