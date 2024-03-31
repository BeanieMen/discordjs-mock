# Discord.js v14 Mocks

This repository contains mocks for the [Discord.js](https://discord.js.org) library.

I created this repository because I wanted to write tests for my Discord bots, but I didn't want to use the real Discord.js library. I wanted to use a mock library that would allow me to test my code without having to worry about the Discord API. I also wanted to be able to test my code without having to worry about the Discord API.

I found a repository that had this as a good base but I wanted more control over the dependencies and the mocks. So I created this repository.

The source was: [AnswerOverflow](https://github.com/AnswerOverflow/AnswerOverflow) and their [Discord.js Mocks](https://github.com/AnswerOverflow/AnswerOverflow/tree/main/packages/discordjs-mock)

## Example

The example below is using DJS. This is just an example and you can use any library you want to create the mocks.

```ts
/* eslint-disable @typescript-eslint/no-explicit-any */
import {vi} from "vitest"
import {
  mockClientUser,
  mockGuild,
  mockGuildMember,
  mockTextChannel,
  mockUser,
} from '@beanieman/discordjs-mock';
import { Client, Guild, TextBasedChannel, TextChannel, User } from 'discord.js';

export class Mock {
  private client!: Client;
  private guild!: Guild;
  private channel!: TextChannel;
  private user!: User;

  getClient(withBots: boolean = false): Client {
    if (withBots) {
      const botUser = mockUser(this.client, { bot: true });
      mockGuildMember({
        client: this.client,
        user: botUser,
        guild: this.guild,
      });
    }
    return this.client;
  }
  getUser(): User {
    return this.user;
  }
  getGuild(): Guild {
    return this.guild;
  }
  getChannel(): TextBasedChannel {
    return this.channel;
  }
  constructor() {
    this.mockClient();
    this.mockGuild();
    this.mockUser();
    this.mockChannel();
  }

  private mockClient(): void {
    this.client = new ClientX({ intents: [], guards: [] });
    mockClientUser(this.client);

    this.client.login = vi.fn(() => Promise.resolve('LOGIN_TOKEN'));
  }

  private mockGuild(): void {
    this.guild = mockGuild(this.client);
  }
  private mockChannel(): void {
    this.channel = mockTextChannel(this.client, this.guild);
  }

  private mockUser(): void {
    this.user = mockUser(this.client);
  }
}
```
