rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {

    match /weddingEnquiries/{docId} {
      // Anyone may create, but only with the two expected fields
      // and sane size limits. No public reads, edits, or deletes.
      allow create: if
        request.resource.data.keys().hasOnly(['name', 'message', 'createdAt'])
        && request.resource.data.name is string
        && request.resource.data.name.size() >= 1
        && request.resource.data.name.size() <= 80
        && request.resource.data.message is string
        && request.resource.data.message.size() >= 1
        && request.resource.data.message.size() <= 2000;

      allow read, update, delete: if false;
    }
  }
}