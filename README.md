Panda Public API Docs
========

Setting up your environment
-----------------------

Docker is the recommended way to work with this repository, as we can use slate's official Docker image and not worry
about dependencies, however if you want to work with the project natively `rbenv` is recommended.

Testing Locally
-----------------------

From the root of the repo:
```
> docker run --rm --name slate -p 4567:4567 -v $(pwd)/source:/srv/slate/source slatedocs/slate serve
```

Then just visit http://localhost:4567 in your browser!

Upgrading Slate
-----------------------

Follow the instructions on [Slate's GitHub page](https://github.com/slatedocs/slate/wiki/Updating-Slate)

Deploying your changes
-----------------------

Make sure all your changes are committed and then build the project

```
docker run --rm --name slate -v $(pwd)/build:/srv/slate/build -v $(pwd)/source:/srv/slate/source slatedocs/slate build
```

This will create the `build/` folder (don't worry about committing this, it's part of the `.gitignore` file). You can now 
deploy: 


Run on Virtaul machine

```
docker run -d -p "127.0.0.1:4567:4567" --name slate -v $(pwd)/build:/srv/slate/build -v $(pwd)/source:/srv/slate/source slatedocs/slate

