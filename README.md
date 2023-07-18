# redwoodjs-resend-quick-start

- Resend with RedwoodJS Quickstart Guide
  - See: send-with-redwoodjs.mdx
- Example RedwoodJS App `resend-redwoodjs-example`
  - Demonstrates `send` function
    - See: resend-redwoodjs-example/api/src/functions/send/send.ts
  - Demonstrates Service with GraphQL Mutation to create and send or just send and Email
    - See: resend-redwoodjs-example/api/src/services/emails/emails.ts

## GraphQL

```ts
// api/src/graphql/emails.sdl.ts

export const schema = gql`
  type Email {
    id: String
    createdAt: DateTime
    updatedAt: DateTime
    resendId: String!
    to: String
    from: String
    subject: String
  }

  input CreateEmailInput {
    to: String
  }

  type Mutation {
    sendEmail(input: CreateEmailInput!): Email! @requireAuth
  }
`;

// api/src/services/emails/emails
export const sendEmail: MutationResolvers["createEmail"] = async ({
  input,
}) => {
  try {
    const from = "RedwoodJS Mailer Service <onboarding@resend.dev>";
    const to = input.to || "delivered@resend.dev";
    const subject = "hello world from RedwoodJS";

    const data = await resend.emails.send({
      from,
      to,
      subject,
      react: Email(subject),
    });

    return { resendId: data.id, from, to, subject };
  } catch (error) {
    throw new Error(error);
  }
};
```

### Send via Mutation

```graphql
mutation MyMutation {
  sendEmail(input: {}) {
    from
    id
    resendId
    subject
    to
  }
}
```
