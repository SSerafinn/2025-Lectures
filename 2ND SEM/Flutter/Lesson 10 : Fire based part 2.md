# Cloud Firestore with Flutter: Building a Notes App

**Course:** ITE 124 — Hybrid Mobile App Development
**School of Information Technology and Engineering (SITE), St. Paul University Philippines**
**Prerequisite:** The Firebase Auth app from the previous lecture (sign up / sign in / sign out working).

---

## What We Are Building Today

We're going to turn that "You are signed in!" home screen into a real, working **notes app** with full **CRUD** — Create, Read, Update, Delete. Each user will see only *their own* notes, stored in the cloud and synced live.

By the end, your app will:

1. Connect to **Cloud Firestore** (Firebase's database)
2. **Create** a note tied to the logged-in user
3. **Read** that user's notes — updating *live* as they change
4. **Update** (edit) the text of an existing note
5. **Delete** a note
6. Keep every user's notes **private to them** using Security Rules

> **The big continuity:** last lecture you learned that *auth state is a stream*. Today's lesson is the same idea applied to data: **your notes are a stream too.** Firestore pushes you a new event every time the data changes, and a `StreamBuilder` rebuilds the list automatically. You already know this pattern — we're just pointing it at a database instead of the auth state.

---

## Part 0 — Where We Start

You should already have the completed auth app with this structure:

```
lib/
├── main.dart              (Firebase-connected, AuthGate)
└── screens/
    ├── auth_screen.dart    (working sign in / sign up)
    └── home_screen.dart    (shows email + UID + sign out)
```

Today we mostly work in `home_screen.dart` (renaming its job to "show notes"), add a small service class, add one new package, and write a few Security Rules in the console.

---

## Part 1 — What Is Cloud Firestore?

Firestore is Firebase's **NoSQL document database**. Instead of tables and rows, it stores:

- **Collections** — named buckets (e.g. `notes`)
- **Documents** — individual records inside a collection, each with a unique ID
- **Fields** — the key/value data inside a document (e.g. `text`, `ownerId`)

> **Mental model:** a *collection* is like a folder, a *document* is like a file in that folder, and *fields* are the contents of that file. A document can hold text, numbers, booleans, timestamps, and even nested data.

Our design for today:

```
notes (collection)
 ├── (auto-id document)
 │     ├── text:      "Buy load for the demo"
 │     ├── ownerId:   "the logged-in user's uid"
 │     └── createdAt: (server timestamp)
 ├── (auto-id document)
 │     └── ...
```

Every note carries an `ownerId` field equal to the user's `uid`. That single field is what lets us show each user only their own notes — and later, what our Security Rules will enforce.

---

## Part 2 — Create the Database (in the browser)

1. Open your project in **https://console.firebase.google.com**.
2. In the sidebar: **Build → Firestore Database**.
3. Click **Create database**.
4. Choose a **location** closest to your users (e.g. an `asia-southeast` region). *This cannot be changed later*, so pick deliberately.
5. For **security rules**, start in **Production mode** (locked by default). We'll write our own rules in Part 8.

> **Why not "Test mode"?** Test mode leaves the database open to the world for 30 days. We'll write proper rules instead — it's a better habit and the whole point of Part 8.

When it finishes, you'll see an empty Firestore data viewer. Leave this tab open; we'll watch documents appear here, just like we watched users appear in the Auth tab last time.

---

## Part 3 — Add the Firestore Package

In your project directory:

```bash
flutter pub add cloud_firestore
```

That's the only new dependency. `firebase_core` is already initialized from last lecture, so Firestore is ready to use immediately — no extra setup in `main.dart`.

> If you want to confirm, your `pubspec.yaml` dependencies should now include `firebase_core`, `firebase_auth`, **and** `cloud_firestore`.

---

## Part 4 — A Tiny Service Class (clean architecture)

Rather than scatter Firestore calls across the UI, we'll put them in one small class. This keeps the screen readable and is good practice your students should carry forward.

Create **`lib/services/note_service.dart`**:

```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:firebase_auth/firebase_auth.dart';

class NoteService {
  // A reference to the top-level "notes" collection.
  final _notesRef = FirebaseFirestore.instance.collection('notes');

  // The current user's id. Used to tag and filter notes.
  String get _uid => FirebaseAuth.instance.currentUser!.uid;

  // READ: a live stream of THIS user's notes, newest first.
  Stream<QuerySnapshot<Map<String, dynamic>>> myNotes() {
    return _notesRef
        .where('ownerId', isEqualTo: _uid)
        .orderBy('createdAt', descending: true)
        .snapshots();
  }

  // CREATE: add a new note owned by the current user.
  Future<void> addNote(String text) {
    return _notesRef.add({
      'text': text,
      'ownerId': _uid,
      'createdAt': FieldValue.serverTimestamp(),
    });
  }

  // UPDATE: change the text of an existing note.
  // We only touch 'text' — ownerId and createdAt stay as they were.
  Future<void> updateNote(String docId, String text) {
    return _notesRef.doc(docId).update({'text': text});
  }

  // DELETE: remove a note by its document id.
  Future<void> deleteNote(String docId) {
    return _notesRef.doc(docId).delete();
  }
}
```

> **Four Firestore methods give us full CRUD:**
> - `.add({...})` → **C**reate a new document with an auto-generated ID.
> - `.snapshots()` → **R**ead a *live stream* of query results.
> - `.doc(id).update({...})` → **U**pdate specific fields of one document.
> - `.doc(id).delete()` → **D**elete one document.
>
> Note `FieldValue.serverTimestamp()` — instead of trusting the phone's clock, we ask Firebase's servers to stamp the time. This keeps ordering correct across devices and time zones.
>
> **Why `.update()` and not `.set()` for editing?** `.update()` changes only the fields you pass and leaves the rest of the document intact, so `ownerId` and `createdAt` are preserved. `.set()` would overwrite the whole document and wipe them unless you re-supply every field.

---

## Part 5 — Read Notes Live (the Home screen)

Now we rebuild `home_screen.dart` to show the notes list, with an **edit** and a **delete** button on each note. **Replace its entire contents** with this:

```dart
import 'package:flutter/material.dart';
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:firebase_auth/firebase_auth.dart';

import '../services/note_service.dart';

class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  final _noteService = NoteService();
  final _textController = TextEditingController();

  @override
  void dispose() {
    _textController.dispose();
    super.dispose();
  }

  // We'll fill these in during Part 6.
  Future<void> _addNote() async {}
  Future<void> _editNote(String docId, String currentText) async {}

  @override
  Widget build(BuildContext context) {
    final email = FirebaseAuth.instance.currentUser?.email ?? '';

    return Scaffold(
      appBar: AppBar(
        title: const Text('My Notes'),
        actions: [
          IconButton(
            icon: const Icon(Icons.logout),
            tooltip: 'Sign out',
            onPressed: () => FirebaseAuth.instance.signOut(),
          ),
        ],
      ),
      body: Column(
        children: [
          // Small header showing who is logged in.
          Padding(
            padding: const EdgeInsets.all(12),
            child: Text('Signed in as $email',
                style: const TextStyle(color: Colors.black54)),
          ),

          // The live list of notes.
          Expanded(
            child: StreamBuilder<QuerySnapshot<Map<String, dynamic>>>(
              stream: _noteService.myNotes(),
              builder: (context, snapshot) {
                // Waiting for the first batch of data.
                if (snapshot.connectionState == ConnectionState.waiting) {
                  return const Center(child: CircularProgressIndicator());
                }

                // Something went wrong (often a missing index — see notes).
                if (snapshot.hasError) {
                  return Center(child: Text('Error: ${snapshot.error}'));
                }

                final docs = snapshot.data?.docs ?? [];

                if (docs.isEmpty) {
                  return const Center(
                    child: Text('No notes yet. Add one below!'),
                  );
                }

                return ListView.builder(
                  itemCount: docs.length,
                  itemBuilder: (context, index) {
                    final doc = docs[index];
                    final data = doc.data();
                    final text = data['text'] ?? '';
                    return ListTile(
                      title: Text(text),
                      trailing: Row(
                        mainAxisSize: MainAxisSize.min,
                        children: [
                          IconButton(
                            icon: const Icon(Icons.edit_outlined),
                            tooltip: 'Edit',
                            onPressed: () => _editNote(doc.id, text),
                          ),
                          IconButton(
                            icon: const Icon(Icons.delete_outline),
                            tooltip: 'Delete',
                            onPressed: () => _noteService.deleteNote(doc.id),
                          ),
                        ],
                      ),
                    );
                  },
                );
              },
            ),
          ),
        ],
      ),

      // Floating button opens the "add note" dialog (wired in Part 6).
      floatingActionButton: FloatingActionButton(
        onPressed: _addNote,
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

**Run it now:**

```bash
flutter run
```

You'll see "My Notes" with an empty-state message. The list is already *listening* to Firestore — it just has nothing to show yet. The buttons do nothing until the next step.

> **This is the payoff slide from the lecture made real:** the `StreamBuilder` here is structurally identical to the `AuthGate`'s `StreamBuilder` from last lecture. Same widget, same waiting/data pattern — just a different stream.

---

## Part 6 — Create and Edit Notes (one shared dialog)

Adding a note and editing a note both need the same thing: a dialog with a text box. So we write **one** helper, `_showNoteDialog`, and use it for both. **Replace the two empty method shells** (`_addNote` and `_editNote`) with this:

```dart
// Shared dialog for both adding and editing.
// If [initialText] is null we're adding; otherwise we're editing.
Future<String?> _showNoteDialog({String? initialText}) {
  final isEditing = initialText != null;
  _textController.text = initialText ?? '';
  // Put the cursor at the end of the existing text when editing.
  _textController.selection = TextSelection.fromPosition(
    TextPosition(offset: _textController.text.length),
  );

  return showDialog<String>(
    context: context,
    builder: (context) {
      return AlertDialog(
        title: Text(isEditing ? 'Edit note' : 'New note'),
        content: TextField(
          controller: _textController,
          autofocus: true,
          decoration: const InputDecoration(hintText: 'Write your note...'),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Cancel'),
          ),
          FilledButton(
            onPressed: () =>
                Navigator.pop(context, _textController.text.trim()),
            child: Text(isEditing ? 'Save' : 'Add'),
          ),
        ],
      );
    },
  );
}

// CREATE
Future<void> _addNote() async {
  final text = await _showNoteDialog();
  if (text != null && text.isNotEmpty) {
    await _noteService.addNote(text);
    // No setState needed — the StreamBuilder updates itself.
  }
}

// UPDATE
Future<void> _editNote(String docId, String currentText) async {
  final text = await _showNoteDialog(initialText: currentText);
  if (text != null && text.isNotEmpty) {
    await _noteService.updateNote(docId, text);
    // Again, the stream pushes the change back to the list automatically.
  }
}
```

**Run it and add a note.** It appears in the list **instantly** — and you never called `setState`. **Now tap the edit icon** on that note: the dialog opens pre-filled with the current text, and saving updates the row live too.

> Drive the point home for students: *we did not refresh anything* for add, edit, *or* delete. Each operation just writes to Firestore, Firestore pushes a new snapshot, and the `StreamBuilder` rebuilds. This same write-then-auto-update behaviour is what powers chat apps, live dashboards, and multiplayer features.

---

## Part 7 — Verify in the Console

Go to **Firestore Database** in the console and refresh. You should see:

- A **`notes`** collection
- One document per note, each with `text`, `ownerId`, and `createdAt`

Confirm the `ownerId` matches the `uid` you saw in the Authentication → Users tab. **That link between the auth user and the data is the heart of today's lesson.**

Try editing a note in the app → watch the `text` field change in the console. Try deleting one (trash icon) → watch it disappear from the console too.

---

## Part 8 — Security Rules: Keep Notes Private

Right now there's a hidden problem. Our *app* filters by `ownerId`, but nothing stops a malicious client from requesting *all* notes. The fix is **Security Rules** — code that runs on Firebase's servers and cannot be bypassed by the client.

In the console: **Firestore Database → Rules** tab. Replace the rules with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    match /notes/{noteId} {
      // You must be logged in to do anything.
      // You may only read a note that you own.
      allow read: if request.auth != null
                  && request.auth.uid == resource.data.ownerId;

      // You may only create a note if you stamp it with YOUR own uid.
      allow create: if request.auth != null
                    && request.auth.uid == request.resource.data.ownerId;

      // You may only update a note you own, AND you may not change its owner.
      allow update: if request.auth != null
                    && request.auth.uid == resource.data.ownerId
                    && request.auth.uid == request.resource.data.ownerId;

      // You may only delete a note that you own.
      allow delete: if request.auth != null
                    && request.auth.uid == resource.data.ownerId;
    }
  }
}
```

Click **Publish**.

> **Read this carefully with your students — it's the security lesson:**
> - `request.auth` is the identity of whoever is making the request. If it's `null`, they're not logged in → deny everything.
> - `resource.data.ownerId` is the `ownerId` of the document *already in the database* (used for read / update / delete).
> - `request.resource.data.ownerId` is the `ownerId` of the document *being written* (used for create and update).
> - **The client cannot be trusted.** Our Dart code filters by `ownerId` for convenience, but the *rule* is what actually enforces it. This is the principle of least privilege: grant exactly the access needed, nothing more.

> **The update rule has TWO ownership checks — note why:**
> - The `resource.data` check stops you editing a note you don't own.
> - The `request.resource.data` check stops you from re-assigning the note to a different user during the edit.
>
> Checking only the first would let a user hand a note to someone else; checking only the second would let a user overwrite a note they don't own. Both together enforce: *edit only your own notes, and don't change who owns them.*

Your app should keep working exactly as before — because it was already only asking for, editing, and deleting its own notes. The difference is that now it *can't* do anything else, even if someone tampered with the code.

---

## The "Missing Index" Error (Expect This)

The first time you run a query that both **filters** (`where`) and **sorts** (`orderBy`) on different fields, Firestore may throw an error in your console output containing a long URL.

**This is normal.** Firestore needs a *composite index* for that query. The error message includes a clickable link — open it, and it pre-fills the index creation form in the console. Click **Create index**, wait a minute for it to build, and re-run. Done.

> **If the link won't open** (some consoles render it un-clickable), create the index by hand: **Firestore Database → Indexes → Composite → Create index**, then enter Collection ID `notes`, field `ownerId` Ascending, field `createdAt` Descending, and Create. Firestore appends `__name__` automatically — don't add it yourself. Adding the edit feature does **not** change this query, so no new index is needed for update.

> Teaching note: don't hide this from students — show it live. Knowing how to read the error and create the index is a real Firestore skill.

---

## Common Errors & Fixes

| Symptom | Likely cause | Fix |
|---|---|---|
| `Missing or insufficient permissions` | Security Rules deny the request | Check you're logged in; verify the rule matches Part 8 (incl. `allow update`) |
| Edit fails but add works | `allow update` rule missing | Re-publish the rules from Part 8 — they must include the update block |
| `The query requires an index` | Composite `where` + `orderBy` query | Click the link in the error, or create the index by hand (see above) |
| Notes don't appear after adding | `ownerId` not set, or rule mismatch | Confirm `addNote` writes `ownerId`; re-check rules |
| `Null check operator used on a null value` | `currentUser` is null | Ensure the screen only shows when logged in (AuthGate handles this) |
| `createdAt` is null briefly | Server timestamp not yet resolved | Normal — it fills in within a moment; handle null in UI if needed |

---

## Recap

Today you learned to:

- Create a Cloud Firestore database
- Model data as **collections** and **documents**
- Do full **CRUD** with `.add()`, `.snapshots()`, `.update()`, and `.delete()`
- Display live data with a `StreamBuilder` — the *same* pattern as the auth lecture
- Scope data per user with an `ownerId` field
- Enforce privacy with **Security Rules** on the server, including a two-sided ownership check for updates

**Key takeaway:** Firestore data is a stream. You listen to it once and the UI reacts — and security is enforced by the server, never the client.

---

## Looking Ahead

- **Firebase Storage** — attach a photo to each note
- **Offline persistence** — Firestore caches data so the app works without a connection
- **Querying & pagination** — handling large collections efficiently

---

## Quick Exercise (For Students)

1. **Turn it into a to-do list.** Add a `done` boolean field to each note and a checkbox in the `ListTile` that toggles it via `updateNote`-style logic. Remember the `allow update` rule already permits this — but think about whether you'd want to restrict *which* fields can change.
2. **Show the timestamp.** Display the `createdAt` time under each note using `data['createdAt']?.toDate()` (handle the brief null while the server timestamp resolves).
3. *Bonus:* add a confirmation dialog before delete, so an accidental tap doesn't lose a note.
