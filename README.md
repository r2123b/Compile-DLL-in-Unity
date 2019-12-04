# Compile DLL in Unity
Compile C# scripts into DLLs in Unity

Mainly use the file, `mcs.bat`(in Windows) or `mcs` (in MacOS), to help build a dll.

```cs
#if UNITY_EDITOR_WIN
readonly string MCSCompilerPath = Path.Combine(EditorApplication.applicationContentsPath,
                                         "MonoBleedingEdge/bin/mcs.bat");
#elif UNITY_EDITOR_OSX
readonly string MCSCompilerPath = Path.Combine(EditorApplication.applicationContentsPath,
                                         "MonoBleedingEdge/bin/mcs");
#endif
var outputdllName = "your_ouput_DLL_name.dll";
var assemblyFileName = "your_referemce_dll_1.dll,your_referemce_dll_2.dll";
var source = "your_cs_script_1.cs your_cs_script_2.cs";

var process = new Process();
process.StartInfo.UseShellExecute = false;
process.StartInfo.CreateNoWindow = true;
process.StartInfo.RedirectStandardError = true;
process.StartInfo.RedirectStandardOutput = true;
process.StartInfo.FileName = MCSCompilerPath;
process.StartInfo.Arguments = string.Format(
                                @"-target:library -sdk:2 -r:""{0}"" -out:{1} {2}",
                                assemblyFileName, outputdllName, source);
try {
	process.Start();
	string output = process.StandardOutput.ReadToEnd();
	if (output != "")
		UnityEngine.Debug.Log(output);

	string error = process.StandardError.ReadToEnd();
	if (error != "")
		UnityEngine.Debug.LogError(error);
	process.WaitForExit();
} catch (System.Exception e) {
	UnityEngine.Debug.LogException(e);
	return;
}
```
