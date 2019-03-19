# Package Docs Review Starter Kit

The purpose of this starter kit is to provide a template and guidelines for creating and distributing *parcels* of package documentation for review.

You can use **#docs-all** or **#docs-packman-private** on Slack to provide feedback or ask questions about this process or about documenting packages in general.

## About Parcels

This document uses the word *parcel* to mean either the whole set of package documentation or only a subset of it. 

If you are working in MD, there is no easy way to submit your changes for technical review. And especially if you create documentation for a really large feature, such as Cinemachine, ProBuilder, or one of the SRPs, you are probably using a multiple-MD, TOC-based documentation set. 

These look great in the output but they tend to introduce a level of complexity that makes sending out documentation reviews difficult.

## Using Local Packages

Local packages are a Package Manager concept that refers to any package that is stored locally (on your computer). These appear in Package Manager when you add them to your list of packages using either of these methods:

* Add them through the [Add package from disk](https://docs.unity3d.com/Packages/com.unity.package-manager-ui@2.0/manual/index.html#extpkg) button in the Unity Package Manager window.
* Edit your [Project manifest file](https://docs.unity3d.com/Packages/com.unity.package-manager-ui@2.0/manual/index.html#PackManManifestsProject).

Once you add them they appear in the Package Manager window with the same properties as regular packages. What makes them useful is that they don't need to have any code or special content to generate the documentation. 

You can dump your MD files in the root directory and your image files in the `images` subfolder, and as long as you add the [DocTools package](https://docs.unity3d.com/Packages/com.unity.package-manager-doctools@1.0/manual/index.html), you can generate a local version of the documentation.

> ***Note:*** This isn't restricted to bundles of documentation for review: you can point to any local GitLab or GitHub repository and generate the package documentation directly from it. This works well because you can modify your source directly and regenerate immediately.



## Template structure

These are all the files you need in order for Package Manager to recognize it as a package and generate documentation from it:

```none
<your-package-name>
  ├── package.json
  ├── README.md
  ├── LICENSE.md
  └── Documentation~
       ├── your-package-name.md
       └── images
```



### package.json file

The `package.json` file is a simple JSON file where you define the following values:

| Value Name:    | Description:                                                 | Comments:                                                    |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `name`         | The official name of your package. <br />***Note:*** This must be the basename of your MD file. | Mandatory.                                                   |
| `displayName`  | The name as it appears in the Package Manager window.        | Recommended. You can keep the default, but that could make it confusing. |
| `version`      | The version of this package.                                 | Optional, since it won't be registered on a server.          |
| `unity`        | The version of Unity to use this package with.               | Recommended to use the same version.                         |
| `description`  | The description that appears in the Package Manager window.  | Optional. Won't affect the documentation generation          |
| `keywords`     | List of search terms.                                        | Optional.                                                    |
| `dependencies` | List of dependencies on other packages. Leave these blank.   | Do not use.                                                  |

Since this is a local package, you can always just download this and leave the values as they are, just switching out the content (the stuff under the `Documentation~` folder).

This file tells the Unity Package Manager everything it needs to know about how to load a *local* folder as a package, but the most important thing is to get the **name** value right.



### Documentation~ folder

Put the MD files in the `Documentation~` folder and all the images under its subfolder `images`. It's best to stick with that structure because that's what Package Manager expects, regardless of whether you are using a single MD file or the ToC-based setup.

#### Single MD file setup

If your documentation is relatively short, you can use a single MD file. Just as long as it's well-formed markdown, anything goes. Well, almost anything: the Package Manager version of DocFx supports Git-flavoured

> ***Note:*** This is also ideal for the *parcel* approach to documentation reviews. The DocFx table of contents is server-based, so making the generated documentation portable is much easier if there's just one HTML file.

#### ToC-based setup

For larger documentation, you can use multiple MD files organized by an MD-based Table of Contents. This is ideal if you have a lot of structure that you want the user to be able to see and navigate. It is a miniature version of what is accessible through the MDEditor now.

These are the rules for using this structure:

* The ToC file must be named `TableOfContents.md`.

* The very first entry (the launch page) in the ToC must refer to a file named **index.md**. It doesn't matter how you name the rest of them, as long as they are all in the `Documentation~` folder.

* Don't leave any MD files in your folder that match your package name. This can confuse the Package Manager version of DocFx because it assumes you are running a Single MD file setup instead.

* The ToC file uses a bulleted list of links to the MD files (without file extensions), indented to convey structure. You can use either tabs or spaces, but you have to be consistent throughout the ToC file. It should look like this:

  ```markdown
  * [One](index)
  * [Two](two)
  	* [Two and a Half](two-and-a-half)
  	* [Two and Three Quarters](two-and-three-quarters)
  * [Three](three)
  
  ```



So if this were the entire set of documentation, this would be your package file structure:

```none
<your-package-name>
  ├── package.json
  ├── README.md
  ├── LICENSE.md
  └── Documentation~
       ├── TableOfContents.md
       ├── index.md
       ├── two.md
       ├── two-and-three-quarters.md
       ├── two-and-a-half.md
       ├── three.md
       └── images
            ├── one.png
            ├── two.png
            └── three.png
            	etc.
```



### images subfolder

Put your screen grabs and other graphics files in the `<package-root-folder>/Documentation~/images` folder. Package Manager can handle subfolders inside of the `images` folder, but it's best to stick with simplicity. This is because the Package Manager implementation of DocFx has been known to choke on subfolder structures.  