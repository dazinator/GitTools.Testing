# GitTools.Testing
Makes it easy to automate git for testing libraries which interact with git

## Fixtures
The main purpose for this library, providing test fixtures which wrap a temporary git repository making it super easy to create test scenarios. For example

``` csharp
using (var fixture = new GitTools.Testing.EmptyRepositoryFixture())
{
    fixture.MakeACommit();
    fixture.MakeACommit();
    fixture.MakeATaggedCommit("1.0.0");
    fixture.BranchTo("develop");
    fixture.MakeACommit();
    fixture.Checkout("master");
    fixture.MergeNoFF("develop");
}
```

Other fixtures:

 - `LocalRepositoryFixture`
 - `RemoteRepositoryFixture` - Creates a remote repository (locally) then clones it giving you access to both to simulate remote repositories.
 - `BaseGitFlowRepository` - Repository setup for gitflow with develop already checked out

### Sequence diagrams
For each of the methods on the fixture, the same extension methods exist on `IRepository`, the difference is that any methods called on the fixture will automatically record that action in a sequence diagram.

When you dispose the fixture it will automatically log to [LibLog](https://github.com/damianh/LibLog/wiki#transparent-logging-support) a [PlantUML Sequence Diagram](http://plantuml.com/sequence.html), which you can paste into a tool like [CodeUML](http://www.codeuml.com/), [SeedUML](https://seeduml.com) or [PlantText](http://www.planttext.com/planttext) to render.

The above test produces

```
@startuml
master -> master: commit
master -> master: commit
master -> master: commit
master -> master: tag 1.0.0
create participant develop
master -> develop: branch from master
develop -> develop: commit
develop -> master: merge
@enduml
```

Paste that into one of the tools above to see the result!

## Other API's
`Generate` - Makes it easy to generate fake signatures
`VirtualTime` - When testing git you can do lots of operations during the same second, this messes git up. VirtualTime increments the time by a minute each time you access `.Now`

And a number of other libgit2sharp extension methods which make the api simpler for your tests.