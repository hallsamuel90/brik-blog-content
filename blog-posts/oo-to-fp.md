# There and Back Again: Refactoring OO to FP

Functional programming(FP) seems to be completely in vogue these days. While I do think FP has many benefits, I often have a hard time with what sometimes seems to me a dogmatic comparison that FP is superior than object oriented (OO) programming.

Contrary to popular belief, I think that OO and FP are closer together than they might appear. At least this seems to be particularly true if the OO code is written with SOLID design principles in mind.

In this article we are going to exploring a refactoring from SOLID object oriented(OO) code to a more functional programming (FP) style using Typescript. In addition to the “how-to” aspect, we’ll look at each refactoring from a testability perspective. I find it is a good gauge of code quality. If its easy to test, there is a high probability that there is not a bunch of funky state or hidden dependencies. 

Without further ado…. lets refactor!

For this example we will use a very *very* simplified bank account example. We’ll have an `Account` domain object and our use case is opening a new account.

```typescript
interface Account {
  id: string;
  name: string;
  accountStatus: 'OPEN' | 'CLOSED';
}

interface AccountDao {
  save: (account: Account) => Promise<Account>;
}

class AccountService {
  constructor(readonly accountDao: AccountDao) {}

  public async openAccount({
    id = uuid(),
    name,
  }: {
    id?: string;
    name: string;
  }) {
    const account: Account = { id, name, accountStatus: 'OPEN' };

    return this.accountDao.save(account);
  }
}
```

As you can see in this example, this is pretty typical SOLID code. We have some stateless service class that contains the business rules for our use case, and we hold a dependency on our data layer to be able to persist our account information. This is easily testable since we can inject a fake implementation using an in-memory database or mock.

In our first refactoring to FP, we need to actually make this a function. And as they say, “a closure is a poor man’s object”. So lets turn this into a functional closure.

```typescript
export const accountService = (accountDao: AccountDao) => {
  const openAccount = ({
    id = uuid(),
    name,
  }: {
    id?: string;
    name: string;
  }) => {
    const account: Account = {
      id,
      name,
      accountStatus: 'OPEN',
    };

    return accountDao.save(account);
  };

  return { openAccount };
};
```

Are we functional yet? Not quite. We could still potentially keep private state in this iteration, so lets remove the closure and bring in a higher-order function.

```typescript
export const openAccount = ({
  id = uuid(),
  name,
  saveAccount,
}: {
  id?: string;
  name: string;
  saveAccount: AccountDao['save'];
}) => {
  const account: Account = {
    id,
    name,
    accountStatus: 'OPEN',
  };

  return saveAccount(account);
};
```

Hey this is pretty cool, we are passing in the dependency directly to the function, we factored out ability to keep state in the closure and its testable all the same. Its feels like an interface with one method and a built in constructor. I digg it.

Still, there is work to do. Can we factor out the dependency all together? First we can take the creating of the account object and extract it to its own function. 

```typescript
export const createAccount = ({
  id = uuid(),
  name,
}: {
  id?: string;
  name: string;
}): Account => ({
  id,
  name,
  accountStatus: 'OPEN',
});
```

Notice that the `createAccount` function is now pure. And instead of depending on the interface, we can just write our `saveAccount` function implementation directly. 

```typescript
export const saveAccount = async (
  account: Account
): Promise<Account> => {
  await fs.promises.writeFile(
    '/accounts-store/accounts.txt',
    JSON.stringify(account)
  );

  return account;
};
```

Lastly we can compose the two to satisfy our use case.

```typescript
export const openAccount = ({
  id = uuid(),
  name,
}: {
  id?: string;
  name: string;
}): Promise<Account> => saveAccount(createAccount({ id, name }));
```

But wait, how is this testable!? We are unable to inject our fake `dao` into the function. The answer here is that we _do not_ unit test the composition. Instead we unit test the pure parts which is very straight forward. In order to test the entire composition, we would need an integration test (a true testament to the name).

In the end, maybe the goal is perhaps not the decision of OO or FP, but more so of stateless programming with clear responsibilities and limited coupling. 

Like most things in life, its not all black and white. Notice that all these refactorings were viable from the start. Each is stateless, testable, and has clear responsibilities! The main difference here is dependency management by using dependency inversion or dependency rejection.

Perhaps we can conclude that the balance lies somewhere in the middle. Personally, I have a preference towards the higher order function refactoring. It seems to have the best of both worlds in that it: 

- Avoids the spaghetti that can come along with classes and closures
- Doesn’t make things so fine grained that its hard to keep track of (functional composition)

Maybe we can invent a new paradigm called FOOP? Thanks for reading!