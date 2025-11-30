# Workspace

## Overview

- IDE: IntelliJ IDE
- Java SDK: 21
- Java Language Level: 21 LTS
- Node Version: 22.13.0
- Node Package Manager: pnpm

## Setup

### Clone source code

1. Navigate to your folder in which you locate your development projects
2. Create root folder: `mkdir gourmetgate`
3. Navigate there: `cd gourmetgate`
4. Clone source code repository: `git clone https://github.com/gourmetgate/gourmetgate.git`

### IntelliJ Setup

1. Install the Eclipse Scout IntelliJ Plugin
2. Start IntelliJ and select the root folder, not the one you have cloned
3. Choose Java SDK and Language Level 21 in Project Settings
4. Set correct Node Version in `Settings > Languages & Frameworks > Node.js`: >=22.13.0 and pnpm package manager
5. Because of
   this [IntelliJ Issue](https://youtrack.jetbrains.com/projects/IDEA/issues/IDEA-355457/Eclipse-ECJ-compiler-does-not-support-Java-21-and-throws-an-error),
   download latest Eclipse Compiler for Java (ECJ) from
   the [official download page](https://archive.eclipse.org/eclipse/downloads/drops4).
6. Navigate to `Settings > Build, Execution, Deployment > Compiler > Java compiler`
7. Use compiler: Eclipse
8. Path to EJC batch compiler tool: Select the location of your downloaded JAR file

### Clone docdump

1. Navigate to your root folder
2. Clone source code repository: `git clone https://github.com/gourmetgate/docdump.git`
3. Open cloned folder with IntelliJ to edit documentation
4. Install the [Mermaid](https://plugins.jetbrains.com/plugin/20146-mermaid) plugin to render diagrams in markdown
5. Install the [OpenAPI](https://plugins.jetbrains.com/plugin/14837-openapi-swagger-editor) plugin to have rendered
   preview of the OpenAPI documentation

