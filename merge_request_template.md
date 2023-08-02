## Description

Describe your changes and their intent. Including the intent will help reviewers determine if your request aligns with the original intent of the image that it addresses. Make note of any omissions, or particular reasons why you did something in a particular way. Even though this should be in your code comments too, it still helps a lot for reviewers' understanding to explain in the description of the request.

## Reviewer instructions

Describe what you want from this review. You can use `@name` to give different reviewers different roles. For example, you might get one person to ensure it fits with the architecture of the rest of the software, another to check the accuracy of the calculations, and have others on the MR as an observer so that they can keep abreast of the status of a change that may impact them.

## Checklist:
- [ ] Branch is up to date with main
- [ ] CI tests pass
- [ ] Tests of new functionalities added where relevant
- [ ] Documentation updated where relevant (methods have docstrings, new classes, functions etc have Doxygen strings, etc)
- [ ] Examples in the repository updated where relevant
- [ ] `README.md` updated where relevant
- [ ] `CHANGELOG.md` updated where relevant, noting in particular any breaking changes
