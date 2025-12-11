# Rikitraki Backend Update

It has been a while since my last update, so I wanted to share some news about what has been happening behind the scenes. Rikitraki has been running quietly and reliably since 2016, and over the past few months I’ve been working on something that has been on my mind for a long time: a full refresh of the backend.

The original system — a Node/Express app running on a dedicated VM with MongoDB — served amazingly well for almost a decade. But the versions of everything (Linux, MongoDB, Node, you name it) were getting old enough that upgrading in place was becoming more trouble than it was worth. So I finally took the plunge and rebuilt the whole backend.

## What Changed

Rikitraki is now running on a completely serverless architecture. All backend logic has been moved to AWS Lambda, and the database has been migrated to DynamoDB. No more VM maintenance, no more database upgrades, no more worrying about keeping the environment alive. Everything scales automatically and requires almost no operational overhead.

One important note: the API remains exactly the same. The new backend simply slots in behind the same interface.

## Why This Was Needed

After so many years of uninterrupted operation, the old setup was showing its age. I had been postponing the upgrade for a while because it was a fairly big project, but it became clear that a clean rebuild would be the best long‑term solution. Now that it’s done, the system is much easier to maintain and better positioned for future improvements.

## Looking Ahead

This backend revamp is actually the first step toward something bigger. I’m currently working on a redesign of the Rikitraki front end, and having a modern backend makes that process much smoother. The new front end is still in the design phase, but I’m excited about where it’s heading.

## Wrapping Up

Rikitraki has always been a personal project that I enjoy working on, and this upgrade feels like a good reset for the next chapter. The site continues to run as usual, but now with a much more solid foundation underneath it.

Since this is a big change under the hood, there might still be a few bugs lurking around. If you notice anything odd, I would really appreciate it if you could report it here or directly in the [Rikitraki GitHub project issues](https://github.com/jimmyangel/rikitraki/issues).

That’s it for now. More updates will come as the front‑end work progresses.
