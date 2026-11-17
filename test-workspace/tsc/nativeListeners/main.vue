<script setup lang="ts">
import { exactType } from '../shared';

async function load() {}

type Listeners = __VLS_NativeListeners<{
	onSimple?: (payload: number) => void;
	onRequired: (payload: number) => void;
	'on-kebab'?: (payload: number) => void;
	onOverloaded?: { (payload: number): void; (payload: string): void };
	onGeneric?: <T>(payload: T) => void;
	onThis?: (this: Window, payload: number) => void;
	onValue?: (payload: number) => boolean;
	onlowercase?: (payload: number) => void;
	title?: string;
}>;

// Only listeners of the form `(payload: P) => void` also accept a function that returns a promise.
exactType({} as keyof Listeners, {} as 'onSimple' | 'onRequired' | 'on-kebab');
exactType(
	{} as Listeners['onSimple'],
	{} as ((payload: number) => void) | ((payload: number) => Promise<unknown>) | undefined,
);
exactType(
	{} as Listeners['onRequired'],
	{} as ((payload: number) => void) | ((payload: number) => Promise<unknown>),
);
</script>

<template>
	<!-- Vue catches the promise that a native listener returns. -->
	<button @click="load"></button>
	<button :onClick="load"></button>
	<dialog @close="load"></dialog>
	<button @click="exactType($event, {} as PointerEvent)"></button>
	<button @click="e => exactType(e, {} as PointerEvent)"></button>
</template>
