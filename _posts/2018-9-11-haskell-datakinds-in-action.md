---
layout: post
title: Haskell DataKinds In Action
---

It is said that an example is worth a thousand words. Having that in mind, I want to show you what for [DataKinds extension](https://downloads.haskell.org/~ghc/7.8.4/docs/html/users_guide/promotion.html) is useful concerning one real issue.

Supposing you have three variables that are assigned to some URL, file, and directory, and we want them to be passed to a function providing that it can ensure that URL, file, directory are from the same article.

The following is an example of such an issue:

{% highlight haskell %}
{-# LANGUAGE DataKinds, KindSignatures, OverloadedStrings #-}

module Lib
    ( someFunc
    ) where

import qualified Data.Text    as T
import qualified Data.Text.IO as TIO

data AdvertTypes =
      RentPremises
    | RentSellLand
    deriving Show

newtype LotsUrl (t :: AdvertTypes) =
  LotsUrl { unLotsUrl :: T.Text } deriving Show

newtype LotsFile (t :: AdvertTypes) =
  LotsFile { unLotsFile :: T.Text } deriving Show

newtype LotsDir (t :: AdvertTypes) =
  LotsDir { unLotsDir :: T.Text } deriving Show


someFunc :: IO ()
someFunc =
  startOutDownloading lotsUrlLand lotsFileLand lotsUrlPremises

startOutDownloading :: LotsUrl t -> LotsFile t -> LotsDir t -> IO ()
startOutDownloading (LotsUrl url) (LotsFile file) (LotsDir dir) = do
  TIO.putStrLn $ "Downloading: " <> url
  TIO.putStrLn $ "Writing to file: " <> dir <> "/" <> file

lotsUrlLand :: LotsUrl RentSellLand
lotsUrlLand = LotsUrl "http://RentSellLand.url"

lotsFileLand :: LotsFile RentSellLand
lotsFileLand = LotsFile "RentSellLand.file"

lotsDirLand :: LotsDir RentSellLand
lotsDirLand = LotsDir "/var/RentSellLand"

lotsUrlPremises :: LotsUrl RentPremises
lotsUrlPremises = LotsUrl "http://RentSellLand.url"

lotsFilePremises :: LotsFile RentPremises
lotsFilePremises = LotsFile "RentSellLand.file"

lotsDirPremises :: LotsDir RentPremises
lotsDirPremises = LotsDir "/var/RentSellLand"
{% endhighlight %}

You can see three types - LotsUrl, LotsFile, and LotsDir - that are using phantom type variables of AdvertTypes kind. Take a second look at the definition of the lotsUrlLand function; it returns value of LotsUrl RentSellLand type, and this time phantom type variable is of RentSellLand [promoted type](https://downloads.haskell.org/~ghc/7.8.4/docs/html/users_guide/promotion.html).

The main aim is accomplished by means of the startOutDownloading function. It has one type variable 't', and since this variable is distributed to all three arguments, it implies that all that 't' must be of the same type regardless of whether 't' is promoted type or not.

You can see it by your self if you change, for example, lotsUrlLand to lotsUrlPremises in line where startOutDownloading is called.

This issue that I described above is just one from a variety of ways as to how DataKinds can help you out.
