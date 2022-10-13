# Dev Process

I am going to explain a typical development process for how to take a feature from idea to production.

At one time there could be up to a dozen developers working on a project. So we need to have a process that is easy to follow and is consistent. This is so that we can all be working on the same of different features at the same time and avoid try and avoid conflicts.

> Remember that there isn't any way to fully avoid conflicts in your code, they will always come up. So following the same process will help to reduce the number of conflicts and make it easier to resolve them.

In this example we are going to pretend that we are a part of a team of multiple developers all working on the same codebase. Each person would have their own feature they are working on.

## Assumptions

I am assuming that you have a basic understanding of the following:

- Git
- Github
- Javascript
- React
- Jest
- React Testing Library

We are also not going to worry about responsive design and we are going to assume that the project is going to be a desktop only application.

## Setup

If you want to follow along then we start we are just going to be using create-react-app (CRA) to get a basic react app up and running.

```bash
npx create-react-app my-app
cd my-app
```

Normally I would be using yarn but I am going to use npm for this example. I would also normally use typescript but so we can keep it simple I am going to just use javascript.

For this example we are also going to be using tailwind for our styling. This is not a requirement but it is just to get some styling in place.

```bash
npm install -D tailwindcss
npx tailwindcss init
```

configure your tailwind config file to compile to the src/\* folders

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ['./src/**/*.{js,jsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

open your `src/index.css` file and replace it with the following

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

We are now going to remove some files and do some small edits to the code.
Files to remove

- `src/App.css`
- `src/App.test.js`
- `src/logo.svg`
- `src/reportWebVitals.js`

You will have to remove the imports for these files in the right places.

Lets do a commit here

```bash
git add .
git commit -m "initial commit"
```

### Push to Github

Go to github and create a new repository. Then push your code to github.

```bash
git remote add origin <Your Github Repo URL>.git
git branch -M main
git push -u origin main
```

This will push your code to github and rename your master branch to main (this is the new default branch name for github).

Before we continue we are going to just make a new branch called `dev`, we will go over this later. We are going to push it up to github as well.

```bash
git branch dev
git push -u origin dev
```

Now we are good to go and you can start your app

```bash
npm start
```

## Feature

In this example we are going to just be building out our header component.
Ideally we would have a design for this component but for this example we are just going to be building it out.

## Branching

We use git flow for our branching strategy. The main thing is that we are using a feature branching instead of a main branch. This is because we use a main branch for our production code and we use a feature branch for our development code.

### Branch Protection

You can add some restrictions to your main branch so that you can only merge in code that has been reviewed and approved. This is a good way to make sure that you don't accidentally push bad code to production. You can do this by adding branch protection rules to your main branch on Github. Go to your repo on github and click on settings. Then click on branches and then add branch protection rules. You can add rules for who can push to the branch, who can merge to the branch, and who can approve pull requests. You can also add rules for what checks need to pass before a pull request can be merged. I would recommend adding checking these rules

- Require a pull request before merging
  - Then Select Require approvals and set how many reviewers you want to require
- Require status checks to pass before merging
- Require conversation resolution before merging

Normally I would also select `Do not allow bypassing the above settings` but for this example we are going to leave it unchecked as you cant do a code review on your own code. If you are working in a team I would recommend checking this so administers can't bypass the rules.

### Starting the Feature

Before starting a new feature we need to make sure that we are up to date with the latest code from the feature branch. We do this by running the following command.

```bash
git checkout main
git pull origin main
```

This will pull the latest code from the main branch and we will use that as a base for our new feature branch.

```bash
git checkout -b feature/header
```

This will create a new branch called feature/header and switch to that branch. We want our naming so that we can easily identify what the branch is for. In this case we are creating a header component so we are going to call it feature/header.

We really want to focus on only working on the feature we are working on. We don't want to be working on multiple features at the same time on the same branch. This can cause quite a bit of confusion and make it hard to keep track of what is going on.

## Development

Inside my `src` folder I am going to create a new folder called `components`. Inside this folder I am going to create another folder called `Header`. Inside this folder I am going to create two files. One called `index.js` and one called `header.jsx`.

inside my `header.jsx` file I am going to create a header component with the help of tailwind.

```jsx
export const Header = () => {
  return (
    <header>
      <nav className="bg-white border-gray-200 px-4 lg:px-6 py-2.5 dark:bg-gray-800">
        <div className="flex flex-wrap justify-between items-center mx-auto max-w-screen-xl">
          <a href="/" className="flex items-center">
            <span className="self-center text-xl font-semibold whitespace-nowrap dark:text-white">
              Example Site
            </span>
          </a>
          <div className="flex items-center lg:order-2">
            <a
              href="/login"
              className="text-gray-800 dark:text-white hover:bg-gray-50 focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-4 lg:px-5 py-2 lg:py-2.5 mr-2 dark:hover:bg-gray-700 focus:outline-none dark:focus:ring-gray-800"
            >
              Log in
            </a>
            <a
              href="/getting-started"
              className="text-gray-800 dark:text-white hover:bg-gray-50 focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-4 lg:px-5 py-2 lg:py-2.5 mr-2 dark:hover:bg-gray-700 focus:outline-none dark:focus:ring-gray-800"
            >
              Get started
            </a>
          </div>
        </div>
      </nav>
    </header>
  );
};
```

Now I am going to export this component from my `index.js` file.

```jsx
export { Header } from './header';
```

I personally like this file structure because it makes it easy to import the component. I can just import it from the `components` folder and then I can just import the component from the `Header` folder.

```jsx
import { Header } from './components/Header';
```

Lets go do that inside our `App.js` file. Just replace the current contents with the following.

```jsx
import { Header } from './components/Header';

function App() {
  return (
    <div className="App">
      <Header />
    </div>
  );
}

export default App;
```

From here our feature is functionally done. I am not going to hook it up with react router or anything like that. I am just going to leave it as is.

Now I would go and make a commit.

```bash
git add .
git commit -m "created header component"
```

You want to make sure your commit message is clear and concise. This is so that when you are looking back at your commits you can easily identify what you have done.

## Testing

I am going to be using [Jest](https://testing-library.com/docs/react-testing-library/intro/) and [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) for my testing. I want to make sure that I have a test for every component that I create. This is so that I can make sure that I don't break anything when I am making changes to my code. This will also help my team mates so that if they make changes to the code, they can make sure that they haven't broken anything that will affect other live app. React Testing Library often says

> The more your tests resemble the way your software is used, the more confidence they can give you.

So with our tests we want to make sure we are testing the component in the same way that it will be used in the app. CRA comes with Jest and React Testing Library already installed so we don't need to install anything.

Inside my `src/Header` folder I am going to make a new file called `header.test.jsx`. Inside this file I am going to import the following.

Now you can probably see why I like to have my components in their own folder. I can have everything related to that component in that folder. This includes the component, the test and any other files that I might need.

For now I am just going to write a basic test to make sure that Header renders correctly with all the right information

We are going to be testing that the header renders correctly, and that the links are correct. We are going to be using the `getByRole` method to get the links. We are going to be using the `toHaveAttribute` method to make sure that the links have the correct href attribute.

```jsx
import { render, screen } from '@testing-library/react';

import { Header } from './Header';

describe('Header', () => {
  it('renders the header', () => {
    render(<Header />);
    expect(screen.getByText('Example Site')).toBeInTheDocument();
  });

  it('renders the login link', () => {
    render(<Header />);
    const loginLink = screen.getByRole('link', { name: /log in/i });
    expect(loginLink).toBeInTheDocument();
    expect(loginLink).toHaveAttribute('href', '/login');
  });

  it('renders the get started link', () => {
    render(<Header />);
    const getStartedLink = screen.getByRole('link', { name: /get started/i });
    expect(getStartedLink).toBeInTheDocument();
    expect(getStartedLink).toHaveAttribute('href', '/getting-started');
  });
});
```

It is really important to make sure that you are testing all the different scenarios. This is so that you can make sure that your code is working as expected. It is also easier to write tests as you go rather than wait until the end to write them. As we continue to add more features to our app, we might need to add or edit some of our tests.

Lets check to make sure our test is working.

```bash
npm test src/components/Header/header.test.jsx
```

Ideally we should then see all our tests pass with the green tick. If we see a red cross then we know that something is wrong with our code and we have to fix it.

If you have snapshots you will have to add the `-u` flag to update the snapshots. In our test we don't have any for now.

```bash
npm test src/components/Header/header.test.jsx -u
```

If you want to run your tests in watch mode you can use the `--watch` flag.

```bash
npm test src/components/Header/header.test.jsx --watch
```

If you want to run all your tests, just run the following command.

```bash
npm test
```

Now I am going to make a commit.

```bash
git add .
git commit -m "created header test"
```

For now we are done with our feature. We have created the component and we have created the test. Now we are going to see how we can get it into our main branch.

## Updating your feature branch

Before you actually do a push, it is actually a good idea to make sure that you are up to date with the main branch. This is so that you don't have any merge conflicts when you are trying to push your code. Some of your other team members might have finished their feature and pushed it to the main branch. If you don't pull the latest changes from the main branch, you might have merge conflicts when you are trying to push your code.

```bash
// Move to your current main branch
git checkout main
git pull

// Go back to your feature branch and merge the main branch into it
git checkout feature/header
git merge main
```

Doing this may result in some merge conflicts. If you do have merge conflicts, you will need to resolve them. You can do this by going into the files that have the merge conflicts and fixing them. Once you have fixed the merge conflicts, you can add the files and make a commit. It is important to make sure that you are fixing the merge conflicts correctly. You don't want to be breaking already any of the code which your team members have already done. if you are not sure how to fix the merge conflicts, you can always ask your team members for help to see what they did.

```bash
// If you have some conflicts
git add .
git commit -m "resolved merge conflicts"
```

It is also a good idea to run your entire test suite before you push your code. This is so that you can make sure that you haven't broken anything. CRA provides you with a script to easily run your tests. You can do this by running the following command.

```bash
npm test --watchAll=false
```

> PS. You might want to make some package.json scripts for these if you are planning on using them more often.

If you have any failing tests, you will need to fix them before you can push your code. You either need to fix the tests or make the component match the tests. Once you have fixed the failing tests, you can add the files and make a commit. It is important to make sure that you are fixing the failing tests correctly.

## Pushing

Now that we have finished our feature and made sure our branch is up to date, we are going to push it to our remote repository. We are going to push it to a new branch. This is so that we can make sure that our code is working as expected before we merge it into our main branch.

```bash
git push origin feature/header
```

## Creating a pull request

Now that we have pushed our code to our remote repository, we are going to create a pull request. This is so that we can get our code reviewed by our team members. We are going to create a pull request from our feature branch to our main branch. You can do this via Github, if you were the latest push, then often you will see a banner asking you to create a pull request. If you don't see this button, you can go to the pull request tab and click on the `new pull request` button. You can then select the branch you want to merge into and the branch you want to merge from. Most of the time the base branch will be `main` and your compare branch would be your feature branch. So in our examples its `feature/header`. Click on the `create pull request` button and then you can add a title and description for your pull request. You want to make your title and description clear and describe everything that is changing in this PR. You can then click on the `create pull request` button again.

There is also a good time to add some labels to your pull request. This is so that you can easily identify what the pull request is for. You can add labels by clicking on the `labels` button. You can then select the labels you want to add. You can also add a milestone to your pull request. This is so that you can easily identify what milestone the pull request is for. You can add a milestone by clicking on the `milestone` button. You can then select the milestone you want to add.

## Reviewing

Now that we have created a pull request, we need someone else to review it. This is so that we can make sure that the code is readable and that it is following the best practices. You ideally want someone else to review your code. This is so that you can get a fresh pair of eyes to look at your code. You can do this by going to the pull request tab and clicking on the pull request that you want to review. You can then click `reviewers` title. You can then select the people that you want to review your code.

This is a great time to get someone more senior to review your code. This is so that you can get some feedback on how you can improve your code. You can also ask them to review your code if you are not sure how to fix something. They can give you some tips on how you can fix it. A PR doesn't need to be for finished work either, if you are stuck on a problem, you can create a PR and ask for help. Once you have got an answer you can either use that PR to finish your feature, or close it and start a new one.

## Continuous Integration (CI)

This set is a bit complicated and varies depending on where you work. We aren't actually going to set any of this up but it is a useful thing to include in your projects. Commonly there are a bunch of checks to make sure that the code you have just done meets the standards we have set in the repo. Things that often get checked are

- Linting
- Unit tests
- End to end tests
- Code coverage

If any of these checks fail, you will need to fix them before you can merge your into main. If you didn't merge main back into your branch before you pushed, one of these would probably fail, so thats why we do it before we push.

## Checking against Dev

Even if everything is passing, you should still check your code against what would be on `main`, but you cant use `main`, so thats why we have `dev`. This is because you might have broken something that someone else has done. Before you merge into main you should then check it against a shared branch that all the other devs are also using. This is often called something like `dev`, `testing`, `staging`. At the beginning we created a branch called dev, so we will use that. You can do this by checking out your dev branch and pulling the latest changes.

```bash
git checkout dev
git pull
```

Now that we have the latest changes from dev, we are going to merge our feature branch into it. This is so that we can check that our code is working as expected. We can do this by running the following command.

```bash
git merge feature/header
```

Once again there may be some merge conflicts. Here you have to be a bit more careful as conflicts now often means that you are conflicting with another team members active work. If you have to do any major changes you need to do them in your feature branch and then merge that into dev. If you are not sure how to fix the merge conflicts, you can always ask your team members for help to see what they did.

Here we will just do a quick check of the site to make sure everything is up and running. You might have a separate live environment that you can check against. But here just check it locally. We haven't stopped our server so we can just go to `localhost:3000` and check that everything is working as expected.

### Site working as expected

If the site is working as expected and all our tests are passing, then then we know that we can merge our code into main without any issues.

### Site not working as expected

If for some reason the site is broken or the tests are failing, then we need to fix them before we can merge our code into main, go back to your feature branch and fix the issues. Once you have fixed the issues, you can add the files and make a commit and push it back to your remote repo which will update your pull request. Ask for another review and then check it against dev again.

Once everything is all good then you can continue to merge your code into main. But before that though make sure you push your dev branch up to the remote repo. This is so that other dev's can pull it down and check it out.

```bash
git push origin dev
```

After you have merged your stuff into `dev` then you should tell the rest of your team that it is in there. The best way to do that is to create a new label called `on-dev` or something similar. Add that label to your PR to tell the rest of your team that this PR is currently on remote dev. You can create a new label by going to the labels tab and clicking on the new label button. You can then add a name and a color to your label. Once you have added a name and a color to your label, you can click on the create label button.

## Merging into main

Now that you now that your feature is done, another dev has approved it, all the checks from CI are passing (if applicable) and you have checked it against dev, you can now merge your code into main. There are a few different options to choose from when completing a PR. You can either squash and merge, rebase and merge or merge. We will go through each of these options.

#### Squash and merge

Squashing will take all your commits and squash them into one commit. This is useful if you have a lot of commits that are just fixing things.

#### Rebase and merge

This moves the entire feature branch to begin on the tip of the main branch, effectively incorporating all of the new commits in main. But, instead of using a merge commit, rebasing re-writes the project history by creating brand new commits for each commit in the original branch.

#### Merge

This will just merge your branch into main. This creates a new “merge commit” in the feature branch that ties together the histories of both branches .This is the default option.

I prefer to use either just merge if all my commits are good, or squash and merge if I have a lot of commits.

Click which ever often is best for you in the situation.

Now your branch is merged into main, you can delete your feature branch. Github has a setting that automatically deletes the remote branch once it is merged. You can find this setting in the repo settings. I like to have this on. But you will have to go and manually delete the branch locally. Make sure to go back to your main branch and you cant delete the branch you are currently on.

```bash
git checkout main
git branch -d feature/header
```

## Resetting all the branches

Now you have merged in your feature you have to reset the other branches. First pull down main. Since you did the merge via github your local repo doesn't have the new changes.

```bash
git checkout main
git pull
```

We now want to get `dev` up to date with main too.

```bash
git checkout dev
// Pulling is just to make sure you have the latest changes
git pull
git merge main
git push origin dev
```

There sometimes is a chance that `dev` gets quite out of sync and requires you to do a full reset of it, this requires you to delete the remote branch and then recreate it.

```bash
git checkout main
git pull
git push origin --delete dev
git checkout -b dev
git push origin dev
```

Remember that some of your team members might have had stuff on dev too. This is where those labels we added to the PR come in handy. Go look at the list of PR's in your repo and find any that have the label `on-dev` then just merge those into the new dev branch and push it up.

Now that everything is reset, you can go and pick up the next feature.

## Conclusion

This is a very basic overview of our Dev flow which you can use while developing. There is a bunch of other things that can happen but this is the basic gist of it. I hope this helps you out in your journey to becoming a better developer.
