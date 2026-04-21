---
layout: single
title:  "Transcript Management"
header:
 image: 
 teaser: "2026-04-06-vtt-process-screenshot.jpg"
date: 2026-04-06
categories: 
  - PhD
tags:
  - scripting
  - terminal
  - python
  - interviews
---

Having completed some interviews over Zoom, I found that it was generating a first pass at a transcript for me.
These were largely reasonably accurate, and certainly close enough that I could identify the errors easily.
The problem however, is that the transcripts are in the VTT format. While essentially a text file I didn't want to have to manually go through and remove every time-coded reference. Or indeed every time my full name was listed. 

So after some podering, a bit of trial and error (with the help of Claude Code for the details of Python) I ended up with a command line tool that I can use to manage the initial reorganisation of these files.
The usage is, as a result, nice and straightforward. Providing the command and the file name in terminal `vtt_process "~/filePath/interview.vtt"` results in the transformation script running.

<figure>
  <img src="/images/2026-04-06-vtt-process-screenshot.jpg" alt="A Terminal window on MacOS showing using the zsh command.">
  <figcaption>The zsh command in use in Terminal.</figcaption>
</figure>

Then I get a new file called `interview_edited.txt` in the same directory. Ready to be copied and pasted into my word template.
You can see in the image below, it corrects some common problems in these VTT files. Firstly, it does some basic spelling correction so that I don't have to redo all the work correcting American English to British English spelling. Particularly for things like 'colour' which come up a lot. It also manages to join instances where one person is speaking into a single text block. The VTT files are heavily encoded to time ranges so this was the most significant replacement. 
I also wanted to remove names. My interviews aren't anonymous but I wanted the transcripts to be consistent, using 'I' for interviewer and 'R' for respondent throughout. 

<figure>
  <img src="/images/2026-04-06-transformed-text.jpg" alt="A screenshot of the two text files.">
  <figcaption>The transformed file is on the left. Compared to the original on the right, the removed text and formatting changes can be seen clearly.</figcaption>
</figure>


In my first interview I did this by doing a series of search functions on the text file in BBEdit. But this was slow and annoying. 
The first time I did this it probably took me an hour using BBEdit. The new scripted route, I managed to transform three transcripts in under five minutes. It made this first go at creating a transcript much easier.
It really helped to make the content digestible so I could follow the logic of the conversation when editing.

The actual code for this is hosted as a [Gist on GitHub](https://gist.github.com/aarblaster).
It's two files really, a Python helper that does the real work, and a zsh wrapper that I call from the terminal. The zsh wrapper is very simple:

```
#!/bin/zsh

# Process a Zoom/Teams .vtt transcript for research use.
# Return a count of the number of times speakers were identified as an easy way or doing basic error checking.

function vtt_process() {
	local script="/user/pathname/vtt_process.py" # This should be the path to the script. 
	if [[ -z "$1" ]]; then
		echo "Usage: vtt_process <file.vtt> [--self \"Speaker Name\"]"
		return 1
	fi
	python3 "$script" "$@"
}

```

I have both in my collection of zsh files incorporated into my ~/.zshrc. 
The python code is quite long, mostly because of the spelling checks required, which are all hard coded.
An admittedly inelegant solution, but it works, which is all I really care about.

```
#!/usr/bin/env python3

SPELLING = {
	# -our
	'color': 'colour', 'colors': 'colours', # Carries on...

	# -re
	'theater': 'theatre', # Carries on...

	# -ise/-ize
	'recognize': 'recognise', # Carries on...

	# -yse/-yze
	'analyze': 'analyse', # Carries on...

	# -ence/-ense
	'defense': 'defence', # Carries on...

	# -logue/-log
	'dialog': 'dialogue', # Carries on...
}
```


The core of the processing is done in this block, where each of the defined helpers are called. Crucially this is where the default name is set.
```
#!/usr/bin/env python3

def main():
	parser = argparse.ArgumentParser(
		description='Process a Zoom/Teams .vtt transcript for research use.'
	)
	parser.add_argument('input', help='Path to the .vtt file')
	parser.add_argument(
		'--self',
		dest='self_name',
		default='Rose Tyler', # Change the default name here so you don't need to input your name every time.
		metavar='NAME',
		help='Speaker name to label as I (default: "Rose Tyler")',
	)
	args = parser.parse_args()

	input_path = Path(args.input).expanduser().resolve()
	if not input_path.exists():
		print(f'Error: file not found: {input_path}', file=sys.stderr)
		sys.exit(1)

	content = input_path.read_text(encoding='utf-8')

	blocks = parse_vtt(content)
	blocks = merge_consecutive_speakers(blocks)
	blocks = assign_labels(blocks, args.self_name)
	blocks = [(speaker, apply_spelling(text)) for speaker, text in blocks]

	output = '\n\n'.join(f'{speaker}: {text}' for speaker, text in blocks)

	# Strip .vtt and any preceding .transcript suffix before naming the output.
	base = input_path.stem  # e.g. "foo.transcript"
	if base.endswith('.transcript'):
		base = base[: -len('.transcript')]
	output_path = input_path.parent / (base + '_edited.txt')
	output_path.write_text(output + '\n', encoding='utf-8')

	print(f'Written to: {output_path}')
	speaker_counts = {}
	for speaker, _ in blocks:
		speaker_counts[speaker] = speaker_counts.get(speaker, 0) + 1
	for speaker, count in sorted(speaker_counts.items()):
		print(f'  {speaker}: {count} turn(s)')


if __name__ == '__main__':
	main()

```

The challenge here was to make sure that the bits of text near the individual speaker blocks were managed properly. This took quite a bit of trial and error.
As you can see above, the code allows you to specify the interviewer name when executed, but if none is provided, it will default to the coded default. Obviously in my copy the default isn't `Rose Tyler`.

All the code is under MIT Licence so go nuts and use it if it's looks useful.

The [python gist is here](https://gist.github.com/aarblaster/10d7e3a10fea051e3bd0ea6b50521a00) and the [zsh wrapper is uploaded here.](https://gist.github.com/aarblaster/d147b1b886584def6dc6e203f5f90581)
Both are stored as GitHub Gists so they are easily viewable and can be downloaded and copied as you wish!
