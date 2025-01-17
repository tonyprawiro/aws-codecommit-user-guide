# Create a Git Tag in AWS CodeCommit<a name="how-to-create-tag"></a>

You can use a tag to mark a commit with a label that helps other repository users understand its importance\. To create a tag in a CodeCommit repository, you can use Git from a local repo connected to the CodeCommit repository\. After you have created a tag in the local repo, you can use git push \-\-tags to push it to the CodeCommit repository\. 

For more information, see [View Tag Details](how-to-view-tag-details.md)\.

## Use Git to Create a Tag<a name="how-to-create-tag-git"></a>

Follow these steps to use Git from a local repo to create a tag in a CodeCommit repository\.

In these steps, we assume that you have already connected the local repo to the CodeCommit repository\. For instructions, see [Connect to a Repository](how-to-connect.md)\.

1. Run the git tag *new\-tag\-name* *commit\-id* command, where *new\-tag\-name* is the new tag's name and *commit\-id* is the ID of the commit to associate with the tag\.

   For example, the following command creates a tag named `beta` and associates it with the commit ID `dc082f9a...af873b88`:

   ```
   git tag beta dc082f9a...af873b88
   ```

1. To push the new tag from the local repo to the CodeCommit repository, run the git push *remote\-name* *new\-tag\-name* command, where *remote\-name* is the name of the CodeCommit repository and *new\-tag\-name* is the name of the new tag\. 

   For example, to push a new tag named `beta` to a CodeCommit repository named `origin`:

   ```
   git push origin beta
   ```

**Note**  
To push all new tags from your local repo to the CodeCommit repository, run git push \-\-tags\.  
To ensure your local repo is updated with all of the tags in the CodeCommit repository, run git fetch followed by git fetch \-\-tags\.

For more options, see your Git documentation\.