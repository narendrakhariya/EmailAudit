<script lang="ts">
  export let recipients: string[]
  const maxChars = 35
  const { trimmedEmails, hiddenCount, hiddenEmails } = trimEmails(
    recipients,
    maxChars
  )

  function trimEmails(emails: string[], maxChars: number) {
    const totalEmails = emails.length
    let trimmedEmails = []
    let hiddenEmails = ''
    let width = 0

    for (let i = 0; i < totalEmails; i++) {
      let email = emails[i]
      let emailWidth = email.length * 10 // 10 is an arbitrary width value for each character

      if (totalEmails === 1) {
        // If there is only one recipient
        trimmedEmails.push(email)
      } else if (width + emailWidth <= maxChars * 10 || i === 0) {
        // If more than one recipient
        trimmedEmails.push(email)
        width += emailWidth
      } else {
        // Append hidden emails to show on bedge mouse hover
        hiddenEmails += email + (totalEmails - 1 !== i ? '\n' : '')
      }
    }

    const hiddenCount = totalEmails - trimmedEmails.length

    return { trimmedEmails, hiddenCount, hiddenEmails }
  }
</script>

<style lang="scss">
  .email-list {
    display: flex;
    justify-content: space-between;
  }
  .trim-emails {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .bedge {
    float: right;
    background-color: $color-primary;
    color: #f0f0f0;
    padding: 2px 5px;
    border-radius: 3px;
  }
</style>

<!--
@component
Receives a `recipients` string [] prop and renders recipients with bedge

- Usage:
  ```tsx
  <RecipientsDisplay {recipients} />
  ```
-->

<div class="email-list">
  <span class="trim-emails">
    {#each trimmedEmails as email}
      {email}{hiddenEmails.length === 0 ? '' : ', '}
    {/each}
    {#if hiddenCount > 0}...{/if}
  </span>
  {#if hiddenCount > 0}
    <span class="bedge" title={hiddenEmails}>+{hiddenCount}</span>
  {/if}
</div>
