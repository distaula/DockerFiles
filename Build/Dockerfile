FROM microsoft/dotnet-framework

ENV chocolateyUseWindowsCompression false

RUN powershell -NoProfile -Command \
    wget 'https://chocolatey.org/install.ps1' -UseBasicParsing -Outfile 'C:\Choco.ps1' ; \
    C:\Choco.ps1 ; \
    del C:\Choco.ps1

RUN powershell -NoProfile -Command \
    choco install -y git ; \
    choco install -y nodejs --version 6.9.4 ; \
    "& 'C:\Program Files\nodejs\npm.cmd' install -g gulp" ; \
    "& 'C:\Program Files\nodejs\npm.cmd' install -g grunt" ; \
    "& 'C:\Program Files\nodejs\npm.cmd' install -g bower"

RUN powershell -NoProfile -Command \
    Invoke-WebRequest -Method Get -Uri https://aka.ms/vs/15/release/vs_BuildTools.exe -OutFile buildtools.exe ; \     
    "& ./buildtools.exe --quiet --wait --add 'MSBuild Tools' --add 'Web Development Build Tools' --add 'Microsoft.Net.Component.4.6.1.SDK' --add 'Microsoft.Net.Component.4.6.1.TargetingPack'" ; \    
    Sleep 5 ; \    
    Get-Process ; \
    Wait-Process buildtools ; \
    "& setx PATH '%PATH%;C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\MSBuild\15.0\Bin\'" ; \
    del buildtools.exe
    
RUN powershell -NoProfile -Command \
    choco install -y NuGet.Commandline ; \
    choco install -y webdeploy ; \
    nuget install -OutputDirectory 'C:\TempNuget' 'MSBuild.Microsoft.VisualStudio.Web.targets' ; \
    mkdir 'C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\MSBuild\Microsoft\VisualStudio\v15.0' ; \
    "& Get-ChildItem -Path 'C:\TempNuget' -recurse VSToolsPath | Get-ChildItem | % {Move-Item $_.FullName -Destination 'C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\MSBuild\Microsoft\VisualStudio\v15.0'}" ; \
    rmdir -ErrorAction SilentlyContinue -Force -Recurse 'C:\TempNuget'

CMD 'C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\MSBuild\15.0\Bin\msbuild.exe'