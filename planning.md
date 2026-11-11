# Planning

## Users

- Readers view articles and newsletters and follow publishers or journalists.
- Journalists create and manage their own articles and newsletters.
- Editors review articles and manage content for their publishers.
- Users register as Readers, Journalists, or Editors with unique emails.
- Editors create publishers and assign registered team members.

## Main features

- Registration, login, and logout.
- Articles with a title, story, author, optional publisher, photo, and caption.
- Publisher-backed articles require approval by their publisher's Editors.
- Journalists publish their own independent articles.
- Editing makes an article unpublished until approved or published again.
- Reader subscriptions and a subscribed newsfeed.
- Newsletters containing approved articles.
- Subscriber emails and API logging after article publication.
- Token-authenticated API with access checks.
- Automated tests.

## Page layout

All pages share a header, navigation, and stylesheet.

The homepage features the newest approved article.
Other recent stories appear beside it on desktop and below it on mobile.

Login and registration use a narrower, centred layout.
Navigation links depend on the user's role.

## Database relationships

- A publisher can have several journalists and editors.
- A journalist or editor can belong to several publishers.
- Each article has one author and an optional publisher.
- A newsletter has one author and can contain several articles.
- Readers can subscribe to several publishers and journalists.

MariaDB stores the application data.

## Quality and security

- Check permissions in views and API endpoints, not just navigation.
- Validate submitted forms and API data.
- Keep passwords and secret settings out of Git.
- Use Django's password handling and CSRF protection.
- Show useful errors and handle empty pages.
- Keep code readable and test successful and rejected requests.

## Decisions

Editors only browse and manage their publishers' articles and newsletters.
Independent articles are published by their Journalist authors.
Newsletter article selections must match the newsletter's publishing scope.
Editors can create newsletters, following the newsletter-specific PDF instruction.

Photos are optional. Weather and manual featured-story selection are deferred.