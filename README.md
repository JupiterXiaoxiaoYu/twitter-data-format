# Twitter Data Structure Documentation

This document describes the data structures used in the Twitter data collection system.

## Data Structure Overview

### 1. User Profile
Basic user information that appears across all data types.

```typescript
interface UserProfile {
    user_id: string;              // Unique Twitter user ID
    user_name: string;            // User's display name
    user_screen_name: string;     // User's Twitter handle (@username)
    user_verified: boolean;       // Whether the account is verified
    user_bio: string;             // User's profile bio
    user_followers_count: number; // Number of followers
    user_following_count: number; // Number of accounts followed
    user_tweets_count: number;    // Total number of tweets
    user_created_at: string;      // Account creation date
    user_location: string;        // User's location
    user_profile_image_url: string; // Profile image URL
    user_display_url: string;     // Website URL from profile
}
```

### 2. Tweet
Represents a single tweet with all associated metadata.

```typescript
interface Tweet {
    // User Profile Information
    ...UserProfile;

    // Tweet Content
    tweet_id: string;            // Unique tweet ID
    text: string;                // Tweet content
    created_at: string;          // Tweet creation time (string format, e.g. "2025-04-14 15:31:23")
    created_at_ts: string;       // Tweet creation time (ISO format, e.g. "2025-04-14T15:31:23+08:00")
    crawl_ts: string;            // Time when the tweet was crawled (ISO format)
    language: string;            // Tweet language code (e.g. "zh", "en", "ja")

    // Engagement Metrics
    likes: number;               // Number of likes
    retweets: number;            // Number of retweets
    replies: number;             // Number of replies
    quotes: number;              // Number of quote tweets
    views: number;               // Number of views

    // Media and Tags
    media_present: boolean;      // Whether the tweet contains media
    media_urls: string[];        // Array of media URLs
    hashtags: string[];          // Array of hashtags
    mentions: string[];          // Array of mentioned users
    source: string;              // Source of the tweet (e.g. "Twitter Web App", "Twitter for iPhone")

    // Tweet Relationships
    is_reply: boolean;           // Whether this is a reply
    is_retweet: boolean;         // Whether this is a retweet
    quoted_tweet_id: string;     // ID of quoted tweet
    conversation_id: string;     // ID of the conversation
    original_tweet_id: string;   // ID of the original tweet
    in_reply_to_status_id: string; // ID of the tweet being replied to
    in_reply_to_user_id: string;   // ID of the user being replied to
    in_reply_to_screen_name: string; // Screen name of the user being replied to
    is_quote_status: boolean;    // Whether this is a quote tweet
    possibly_sensitive: boolean; // Whether the content is sensitive

    // Quoted Tweet Information
    quoted_status_summary?: {
        tweet_id: string;
        text: string;
        user_name: string;
        user_screen_name: string;
        created_at: string;
        created_at_ts: string;
        likes: number;
        retweets: number;
        replies: number;
        quotes: number;
        views: number;           // Number of views of the quoted tweet
    };

    // Retweet Information
    retweeted_status_summary?: {
        tweet_id: string;
        text: string;
        user_name: string;
        user_screen_name: string;
        created_at: string;
        created_at_ts: string;
        likes: number;
        retweets: number;
        replies: number;
        quotes: number;
        views: number;           // Number of views of the retweeted tweet
    };
}
```

### 3. Reply
Represents a reply in a conversation thread.

```typescript
interface Reply {
    // User Profile Information
    ...UserProfile;

    // Reply Content
    tweet_id: string;            // Unique reply ID
    text: string;                // Reply content
    created_at: string;          // Reply creation time (string format)
    created_at_ts: string;       // Reply creation time (ISO format)
    crawl_ts: string;            // Time when the reply was crawled
    language: string;            // Reply language code

    // Engagement Metrics
    likes: number;               // Number of likes
    retweets: number;            // Number of retweets
    replies: number;             // Number of replies
    quotes: number;              // Number of quote tweets
    views: number;               // Number of views

    // Media and Tags
    media_present: boolean;      // Whether the reply contains media
    media_urls: string[];        // Array of media URLs
    hashtags: string[];          // Array of hashtags
    mentions: string[];          // Array of mentioned users
    source: string;              // Source of the reply

    // Reply Relationships
    is_target_user: boolean;     // Whether the reply is from the target user
    tweet_type: string;          // Type of tweet ("raw tweet", "reply", "quote")
    thread_id: string;           // ID of the conversation thread
    position_in_thread: number;  // Position in the thread
    is_root_tweet: boolean;      // Whether this is the root tweet
    reply_depth: number;         // Depth in the reply chain
    in_reply_to_status_id: string; // ID of the tweet being replied to
    in_reply_to_user_id: string;   // ID of the user being replied to
    in_reply_to_screen_name: string; // Screen name of the user being replied to
    conversation_id: string;     // ID of the conversation
    original_tweet_id: string;   // ID of the original tweet
    quoted_tweet_id: string;     // ID of quoted tweet
    is_quote_status: boolean;    // Whether this is a quote tweet
    possibly_sensitive: boolean; // Whether the content is sensitive

    // Thread Statistics
    thread_stats: {
        reply_count: number;         // Total number of replies in the thread
        target_user_replies: number; // Number of replies from the target user
        max_reply_depth: number;     // Maximum depth of replies in the thread
    };
}
```

### 4. Following/Follower
Represents a user's following or follower relationship.

```typescript
interface Following {
    user_id: string;              // User ID
    user_name: string;            // Display name
    user_screen_name: string;     // Twitter handle
    user_verified: boolean;       // Verification status
    user_bio: string;             // Profile bio
    user_followers_count: number; // Number of followers
    user_following_count: number; // Number of accounts followed
    user_tweets_count: number;    // Total number of tweets
    user_created_at: string;      // Account creation date (string format, e.g. "2018-10-17 14:59:23")
    user_created_at_ts: string;   // Account creation date (ISO format, e.g. "2018-10-17T14:59:23+08:00")
    crawl_ts: string;             // Time when the data was crawled (ISO format)
    user_location: string;        // User's location
    user_profile_image_url: string; // Profile image URL
    user_display_url: string;     // Website URL from profile
}
```

## Data Files

The system uses the following JSON files:

1. `user_*_tweet_cleaned.json` - Contains the user's tweets
2. `user_*_replies_cleaned.json` - Contains replies to the user's tweets
3. `user_*_followings_cleaned.json` - Contains the user's following list
4. `user_*_followers_cleaned.json` - Contains the user's followers list
5. `token_*_tweets.json` - Contains tweets related to specific tokens (e.g., BTC)

## Notes

- All timestamps are stored in both string format and ISO format (with `_ts` suffix)
- Media content is stored as an array of URLs
- Boolean flags are used to indicate various states (e.g., `is_reply`, `is_quote_status`)
- Engagement metrics are stored as numbers
- The data structure supports both direct tweets and threaded conversations
