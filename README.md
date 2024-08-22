Installing Rust using rustup. You can install rustup as follows:

macOS or Linux:

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
Windows (64-bit):

Download the Windows 64-bit executable and follow the on-screen instructions.

Windows (32-bit):

Download the Windows 32-bit executable and follow the on-screen instructions.

🐙 Build from Source Code
Installing Leo by building from the source code as follows:

# Download the source code
git clone https://github.com/AleoHQ/leo
cd leo

# Install 'leo'
$ cargo install --path .
Now to use leo, in your terminal, run:

leo
🦁 Update from Leo
You can update Leo to the latest version using the following command:

leo update
Now to check the version of leo, in your terminal, run:

leo --version
🚀 Quick Start

First workshop 
Use the Leo CLI to create a new project

# create a new `hello-world` Leo project
leo new helloworld
cd helloworld

# build & setup & prove & verify
leo run main 0u32 1u32
leo deploy --network tesnet
The leo new command creates a new Leo project with a given name.
The leo deploy command deploys the program to the blockchain so gas fee is required.
The leo run command will compile the program into Aleo instructions and run it.

Congratulations! You've just run your first Leo program.
