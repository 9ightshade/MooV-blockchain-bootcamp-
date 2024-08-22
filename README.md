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

Workshop 1

Transcation id for project one

at1t8huws58ar2qz8yvujnshqvluvdvfjglzalt9ke3nv6nck86fypq3mh7u9

Code used // The 'helloworld_g6npriz.aleo' program. program dante_salvador_blake_hello1.aleo { transition main(public a: u32, b: u32) -> u32 { let c: u32 = a + b; return c; } }

//this allowed the code to to deploy a hello world program identifying the data type required for the public and private info//

//using this leo run main 2u32 3u32 --network testnet

WORKSHOP 2

Transcation id for project two

at146h2d5e060u6v65cal4axjhq8x5nasr9wpnxga08jc0ttfpe7vrszcw5m6

// The 'token_moov_nightshade.aleo' program. program dante_salvador_token1.aleo { // The Token record datatype. record Token { // The token owner. owner: address, // The token amount. amount: u64, } // The mint function initializes a new record with the // specified number of tokens assigned to the specified receiver. transition mint(owner: address, amount: u64) -> Token { return Token { owner: owner, amount: amount, }; } // The transfer function sends the specified number of tokens // to the receiver from the provided token record. transition transfer(token: Token, to: address, amount: u64) -> (Token, Token) {

    // Checks the given token record has sufficient balance.
    // This `sub` operation is safe, and the proof will fail
    // if an overflow occurs.
    // `difference` holds the change amount to be returned to sender.
    let difference: u64 = token.amount - amount;

    // Produce a token record with the change amount for the sender.
    let remaining: Token = Token {
        owner: token.owner,
        amount: difference,
    };

    // Produce a token record for the specified receiver.
    let transferred: Token = Token {
        owner: to,
        amount: amount,
    };

    // Output the sender's change record and the receiver's record.
    return (remaining, transferred);
}
}

// THIS code takes record of transaction carried out then store info about it and calculate what is deducted from the senders account and keeps record of the balance

Workshop 3

Transcation id for project three

at1qqnwjema6nvf279602v4n4s0k3a38ze802vmj2p0tyx4sygrl5pslxnyy4

// The 'moov_dev_aleo_voice.aleo' program. // The 'aleo_voice7' program. program dante_projectwork3.aleo {

//Record for Voice data
record Voice {
    owner: address,
    receiver: address,
    msg: u128,
    hash_msg: u128,
}

//Record for combining both owner and receiver
record CoBind {
    hash_owner: field,
    owner: address, 
    receiver: address,
    bind_hash: field,
}

// struct data for storing user data
struct Messaging {
    messenger: field,
    total_messages: u64,
}

//Mapping for  user data
mapping voice_msg: field => Messaging; 

//send voice transition function with the async await js to update state. has 4 inputs
async transition send_voice (owner: address, receiver: address, msg: u128,  co_bind: field) -> (Voice, Future){
    //hash a message
    let hash: u128 = BHP256:: hash_to_u128(msg);

    //checks if address calling match owner address
    assert(self.caller == owner);

    //checks address calling is not a receiver
    assert_neq(self.caller, receiver);
    
    //hash the owner
    let hash_owner: field = BHP256:: hash_to_field(owner);

    //hash the receiver
    let hash_receiver: field = BHP256:: hash_to_field(receiver);

    //add both [owner and receiver] hash
    let add_hash : field = hash_owner + hash_receiver ;

    // declaring them into a record -> Voice
    let send_from : Voice = Voice {
        owner: owner,
        receiver: receiver,
        msg: msg,
        hash_msg: hash
    };
    
    //return the record and update the state onchain with the function name - finalize_send_voice
    return (send_from, finalize_send_voice(owner, hash));

}
//state being updated
async function finalize_send_voice (owner: address, hash: u128){
    //hash the owner
    let hash_owner: field = BHP256:: hash_to_field(owner);
    // getting the mapping detail, by default mapping is to -> hash_owner  and 0 total messages
    let total: Messaging = Mapping::get_or_use(voice_msg, hash_owner, Messaging{
        messenger: hash_owner,
        total_messages: 0u64,
    });
    
    //storing/setting the data into the mapping, now the data component will be uopdate to the hash_owner and n total messages
    voice_msg.set(hash_owner, Messaging {
        messenger: hash_owner,
        total_messages: total.total_messages + 1u64,
    });
}

transition combine_hash_owner_receiver(owner: address, receiver: address) -> (CoBind){
    assert_eq(owner, self.caller);
    let hash_owner: field = BHP256::hash_to_field(owner);
    let hash_receiver: field = BHP256::hash_to_field(receiver);
    let add_hash: field = hash_owner + hash_receiver;

    let update_hash: CoBind = CoBind {
        hash_owner: hash_owner,
        receiver: receiver,
        owner: owner,
        bind_hash: add_hash,
    };
    return (update_hash);
}
}

this allowed the info gotten from the sender and reciever to be stored and using updates every detail after transaction
