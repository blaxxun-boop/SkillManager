# Skill Manager

Can be used to easily add new skills to Valheim. Will automatically add config options to your mod and sync the configuration from a server, if the mod is installed on the server as well.
Please note that you still have to implement the effect your skill has yourself.

## How to add skills

Copy the skill icon (a 64x64 image) into an `icons` directory in your project and make sure to set it as an EmbeddedResource in the properties of the asset bundle.

### Merging the DLLs into your mod

Download the SkillManager.dll and the ServerSync.dll from the release section to the right.
Including the DLLs is best done via ILRepack (https://github.com/ravibpatel/ILRepack.Lib.MSBuild.Task). You can load this package (ILRepack.Lib.MSBuild.Task) from NuGet.

If you have installed ILRepack via NuGet, simply create a file named `ILRepack.targets` in your project and copy the following content into the file

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="ILRepacker" AfterTargets="Build">
        <ItemGroup>
            <InputAssemblies Include="$(TargetPath)" />
            <InputAssemblies Include="$(OutputPath)\SkillManager.dll" />
            <InputAssemblies Include="$(OutputPath)\ServerSync.dll" />
        </ItemGroup>
        <ILRepack Parallel="true" DebugInfo="true" Internalize="true" InputAssemblies="@(InputAssemblies)" OutputFile="$(TargetPath)" TargetKind="SameAsPrimaryAssembly" LibraryPath="$(OutputPath)" />
    </Target>
</Project>
```

Make sure to set the SkillManager.dll and the ServerSync.dll in your project to "Copy to output directory" in the properties of the DLLs and to add a reference to it.
After that, simply add `using SkillManager;` to your mod and use the `Skill` class, to add your skills.

The SkillManager also adds two extension methods `GetSkillFactor` and `RaiseSkill` on `Character` and `Skills` classes to get and increase the skill in your code by skill name.

## Example projects

https://github.com/blaxxun-boop/Tenacity

This adds a skill `Tenacity`, which reduces the damage taken slightly. The `tenacity-icon.png` file is in a directory called `icons`.

https://github.com/blaxxun-boop/Building

This adds a skill `Building`, which increases the health of pieces built by you.

https://github.com/blaxxun-boop/Evasion

This adds a skill `Evasion`, which reduces the stamina usage of dodging.
