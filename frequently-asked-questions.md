# Frequently Asked Questions

### Can you provide an example header for me to understand what exactly is signed?

We are hijacking DKIM signatures in order to verify parts of emails, which can be verified on chain via succinct zero knowledge proofs. Here is an example of the final, canoncalized actual header string that is signed by google.com's public key:

`to:"zkemailverify@gmail.com" <zkemailverify@gmail.com>\r\nsubject:test email\r\nmessage-id:<CAOmXgjU78_L7d-H7Wqf2qph=-uED3Kw6NEU2PzSP6jiUH0Bb+Q@mail.gmail.com>\r\ndate:Fri, 24 Mar 2023 13:02:10 +0700\r\nfrom:ZK Email <zkemailverify2@gmail.com>\r\nmime-version:1.0\r\ndkim-signature:v=1; a=rsa-sha256; c=relaxed/relaxed; d=gmail.com; s=20210112; t=1679637743; h=to:subject:message-id:date:from:mime-version:from:to:cc:subject :date:message-id:reply-to; bh=gCRK/FdzAYnMHic55yb00uF8AHZ/3HvyLVQJbWQ2T8o=; b=`

Thus, we can extract whatever information we want out of here via regex, including to/from/body hash! We can do the same for an email body.

### I'm having issues with the intricacies of the SHA hashing. How do I understand the function better?

Use https://sha256algorithm.com/ as an explainer! It's a great visualization of what is going on, and our code should match what is going on there.

### I'm having trouble with regex or base64 understanding. How do I understand that better?

Use https://cyberzhg.github.io/toolbox/ to experiment with conversions to/from base64 and to/from DFAs and NFAs.

### What are the differences between generating proofs (snarkjs.groth16.fullprove) on the client vs. on a server?

If the server is generating the proof, it has to have the private input. We want people to own their own data, so client side proving is the most secure both privacy and anonymity wise. There are fancier solutions (MPC, FHE, recursive proofs etc), but those are still in the research stage.

### “Cannot resolve module ‘fs’”

Fixed by downgrading react-scripts version.

### TypeError: Cannot read properties of undefined (reading 'toString')

This is the full error:

```
zk-email-verify/src/scripts/generateinput.ts:182
const = result.results[0].publicKey.toString();
                                    ^
TypeError: Cannot read properties of undefined (reading 'toString')
```

You need to have internet connection while running dkim verification locally, in order to fetch the public key. If you have internet connection, make sure you downloaded the email with the headers: you should see a DKIM section in the file. DKIM verifiction may also fail after the public keys rotate, though this has not been confirmed.

### How do I lookup the RSA pubkey for a domain?

Use [easydmarc.com/tools/dkim-lookup?domain=twitter.com](https://easydmarc.com/tools/dkim-lookup?domain=twitter.com).

### DKIM parsing/public key errors with generate\_input.ts

```
Writing to file...
/Users/aayushgupta/Documents/.projects.nosync/zk-email-verify/src/scripts/generate_input.ts:190
        throw new Error(`No public key found on generate_inputs result ${JSON.stringify(result)}`);
```

Depending on the "info" error at the end of the email, you probably need to go through src/helpers/dkim/\*.js and replace some ".replace" functions with ".replaceAll" instead (likely tools.js), and also potentially strip some quotes.

### No available storage method found.

If when using snarkjs, you see this:

```
[ERROR] snarkJS: Error: No available storage method found. [full path]
/node_modules/localforage/dist/localforage.js:2762:25
```

Rerun with this: `yarn add snarkjs@git+https://github.com/vb7401/snarkjs.git#24981febe8826b6ab76ae4d76cf7f9142919d2b8`

### I'm trying to edit the circuits, and running into the error 'Non-quadratic constraints are not allowed!'

The line number of this error is usually arbitrary. Make sure you are not mixing signals and variables anywhere: signals can only be assigned once, and assigned to other signals (not variables), and cannot be used as parameters in control flow like for, if, array indexing, etc. You can get versions of these by using components like isEqual, lessThan, and quinSelector, respectively.

### Where do I get the public key for the signature?

Usually, this will be hosted on DNS server of some consistent URL under the parent organization. You can try to get it from a .pem file, but that is usually a fraught effort since the encoding of such files varies a lot, is idiosyncratic, and hard to parse. The easiest way is to just extract it from the RSA signature itself (like our generate\_input.ts file), and just verify that it matches the parent organization.

### How can I trust that you verify the correct public key?

You can see the decomposed public key in our Solidity verifier, and you can auto-check this against the mailserver URL. This prevents the code from falling victim to DNS spoofing attacks. We don't have mailserver key rotations figured out right now, but we expect that can be done trustlessly via DNSSEC (though not widely enabled) or via deploying another contract.

### How do I get a Verifier.sol file that matches my chunked zkeys?

You should be able to put in identical randomness on both the chunked zkey fork and the regular zkey generation fork in the beacon and Powers of Tau phase 2, to be able to get the same zkey in both a chunked and non-chunked form. You can then run compile.js, or if you prefer the individual line, just `node --max-old-space-size=614400 ${snarkJSPath} zkey export solidityverifier ${cwd}/circuits/${circuitNamePrimary}/keys/circuit_final.zkey ${cwd}/circuits/contracts/verifier.sol`, where you edit the path variables to be your preferred ones.

The chunked file utils will automatically search for circuit\_final.zkeyb from this command line call if you are using the chunked zkey fork (you'll know you have that fork, if you have a file called chunkFileUtils in snarkJS).

### How do I deal with all of these snarkJS forks?

Apologies, this part is some messy legacy code from previous projects. You use vivekab's fork for keygeneration, sampritipanda's fork for chunked zkey checking on the frontend, and the original snarkjs@latest to get rid of chunking entirely (but you'll need to edit frontend code to not do that). You can do something like \`./node\_modules/bin/snarkjs' inside your repo, and it'll run the snarkjs command built from the fork you're using instead of the global one.

### How do I build my own frontend but plug in your ZK parsing?

zkp.ts is the key file that calls the important proving functions. You should be able to just call the exported functions from there, along with setting up your own s3 bucket and setting the constants at the top.

### What is the licensing on this technology?

Everything we write is MIT licensed. Note that circom and circomlib is GPL. Broadly we are pro permissive open source usage, and really appreciate attribution! We hope that those who derive profit from this, contribute a significant portion of that money altruistically back to help advance and maintain this technology and open source public good.
