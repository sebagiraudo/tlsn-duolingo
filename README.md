# tlsn-duolingo

Prove that you have a x day streak on Duolingo

Little project to learn how to use TLS Notary, trying to prove that you have certain streak on Duolingo.

## How to use it
This project is based on the [Discord md example in TLS Notary](https://docs.tlsnotary.org/quick_start/rust.html).

I'm planning on improving this to be able to run it as a standalone project, but for now the best way to try it out is following these steps:

1. Copy the `/duolingo` folder inside the `/examples` folder in [TLSN-quick-start project](https://github.com/tlsnotary/tlsn.git).
2. Add this to the `Cargo.toml` inside the examples folder:
```
[[example]]
name = "duolingo_streak"
path = "duolingo/duolingo_streak.rs"

[[example]]
name = "duolingo_streak_verifier"
path = "duolingo/duolingo_streak_verifier.rs"
```
3. Follow the steps as the discord example on the [tlsn documentation](https://docs.tlsnotary.org/quick_start/rust.html).


## Motivation
I have a running 1725 day streak on Duolingo. Whether it's the right tool for learning a language or not, it not expected to be thought now. I just enjoy a few minutes every day playing with this app and learning something. 

I was thinking that my streak is entirely in hands of Duolingo. If I want to show someone I have this streak I have to show them my Duolingo app or send them my user for them to check using their app. I want to try some idea of moving that data to blockchain so I can own it. It can also be possible to create some NFT that you had at some point a streak of x amount of days.

The complication is that the streak can be lost any day, so what I'm thinking is generating a proof that at a certain date you had a certain streak. 

I also want to take privacy into account. I use Duolingo with friends and family, but maybe I want to share my streak with someone I don't trust, without revealing my identity or any personal information.

It's still an open problem in my head how or where this could be used. But the thing is I love streaks, so having this data on chain would be amazing even if I can't use it anywhere. However, maybe there are some ideas, like being able to participate in some kind of chats only if you have a streak bigger than x, or comparing your streak with someone you don't know nor trust.


## Steps needed to build this project:
- [X] Check where to obtain data
- [X] Do some TLSN hello world. Evaluate if it's an appropriate tool
- [X] Create some proof generator
- [X] Create some proof verifier or check if I can use the one that already exists.
- [ ] Deploy some app?


## Future work
* Find use cases with this data.
* Allow people to mint a NFT only if they have a streak bigger than X.
* Come up with a better name.
* Think why and how it's useful, what data should remain private, how to avoid one user giving out proofs to everyone.


## Notes
TLS Notary outputs this when creating the proof:
```
Successfully verified that the bytes below came from a session with Dns("example.com") at 2024-09-08 14:14:50 UTC.
Note that the bytes which the Prover chose not to disclose are shown as X.
```
So it's a good option for this idea.


In Duolingo, a get to `https://www.duolingo.com/2017-06-30/users/user_id` retrieves all the data.

If the request is made without authorization some data is returned but if auth is in header we get more data, like email. Streak data is in both, so I don't know if it's needed to have the auth.

The thing is I need to create a proof that the streak is mine. I can create a proof that I know a user that has some streak by sending a request without auth, but it's not as useful as saying I have access to an account that has that streak.


The whole request is way too big for this. Fortunately, you can ask for specific fields:

If we GET `https://www.duolingo.com/2017-06-30/users/{user_id}?fields=streak,email` we will get just streak and email. If no auth is provided there will be only the streak. We need to check if email is in the response so we can guarantee the user has the authorization ergo they have access to the account.
