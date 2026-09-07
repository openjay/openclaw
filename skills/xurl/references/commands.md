# xurl command families

Verify the installed CLI version/help for exact flags. These are the retained
command names from the source package; availability is not account authorization.

| Goal                 | Commands                                                                    |
| -------------------- | --------------------------------------------------------------------------- |
| Read/search          | read POST_ID-or-URL; search QUERY -n N                                      |
| Account/user         | whoami; user HANDLE; auth status; auth apps list                            |
| Timeline             | timeline; mentions; bookmarks; likes                                        |
| Social graph reads   | following; followers; optional --of HANDLE                                  |
| Public writes        | post; reply; quote; delete                                                  |
| Engagement writes    | like/unlike, repost/unrepost, bookmark/unbookmark                           |
| Account graph writes | follow/unfollow, block/unblock, mute/unmute                                 |
| Private messages     | dms for requested read; dm HANDLE MESSAGE for authorized send               |
| Media                | media upload PATH; media status MEDIA_ID; attach returned ID via --media-id |

Global selectors include --app, --auth and --username/-u. Never use secret flags
or verbose/-v. Raw v2 endpoints/methods require the same action scope as shortcuts;
they are not a route around a denied higher-level action. Streaming needs an
explicit duration/budget and stop rule. Use actual response IDs, not example IDs.

An upload can itself transmit private content before a post exists. Verify the
authorized file/audience before upload. On ambiguous mutation, inspect the existing
result by its request/media/post identity before issuing another write.
