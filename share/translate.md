* Create a git branch
* Build and run
    * ui/build
    * ./lila.sh
    * run
* Reproduce the missing translation
* Find it in the code (using hints from the previous step)
* Update appropriate files in translations/sources
    * You can add a test translation in translations/dest
* Generate keys for scala and typescript
    * pnpm run i18n-file-gen
    * ui/build
* Check completion
    * key.scala
    * i18n.d.ts
* Update the code using the key(s)
* Format
    * pnpm run format
    * ./lila.sh
    * scalafmtAll
* Rebuild and run
* Test
* Commit and create a PR