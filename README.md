# ReadMe Synchronization Orb for CircleCI

This is a prototype CircleCI orb that wraps a subset [ReadMe CLI](https://github.com/readmeio/rdme), allowing you to synchronize documents from your CircleCI pipeline.

## Available Jobs

* `readme-sync/sync-docs`
* `readme-sync/sync-spec`

## Required fields

### Project Token

All jobs require a ReadMe project token. This is passed into each sync job. See examples for more details.

For more examples of what can be configured, please see the orb registry page for this orb.

## Things to Note

The ReadMe CLI has a couple caveats when syncing documents.

* Markdown filenames should match their respective ReadMe slug
* Markdown files should contain a metadata tag at the top of file with the following format:

    ```
    ---
    category: some-category-id
    title: Your document title
    ---
    ```
* Existing docs do not need a `category` field, but new documents do. For best practice, just put a category field in every document header. If using an existing category, you can either copy the category from another document in your collection, or you can retrieve it from your ReadMe project via their [Categories API](https://docs.readme.com/developers/reference/categories).

