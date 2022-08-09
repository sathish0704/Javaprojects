import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.Scanner;
import javax.swing.JFileChooser;
import javax.swing.filechooser.FileNameExtensionFilter;

public class Checking {
	public static void main(String[] args) throws FileNotFoundException {
		// initializing required variables
		int i, Numbers, spl, upper, lower, cc;
		Numbers = spl = cc = upper = lower = 0;
		char ch;
		String str = "";

		// Getting the file to be selected via FileChooser
		JFileChooser chooser = new JFileChooser();

		// filter added for text files only
		FileNameExtensionFilter filter = new FileNameExtensionFilter("TEXT FILES", "txt", "text");
		chooser.setFileFilter(filter);

		chooser.showOpenDialog(null);
		File file = chooser.getSelectedFile();
		String file_path = file.getAbsolutePath();

		try {
			str = new String(Files.readAllBytes(Paths.get(file_path)));
		} catch (IOException e) {
			e.printStackTrace();
		}

		// getting size of the file
		long bytes = file.length();

		// setting file size limit
		if (bytes <= 1048576) {
			try (Scanner in = new Scanner(new FileInputStream(file), "UTF-8")) {
				while (in.hasNext()) {
					// reading the data form file & stored in string
					str = in.next();

					System.out.println("Passed String is : " + str);

					for (i = 0; i < str.length(); i++) {
						ch = str.charAt(i);

						// to count the numbers
						if (Character.isDigit(ch)) {
							Numbers++;
						}

						// to count the upper case
						else if (Character.isUpperCase(ch)) {
							upper++;
						}

						// to count the lower case
						else if (Character.isLowerCase(ch)) {
							lower++;
						}

						// to count the control characters
						else if ((ch == '\b') || ch == '\t' || ch == '\n' || ch == '\f' || ch == '\r' || ch == '\"'
								|| ch == '\'' || ch == '\\') {
							cc++;
						}

						// to count remaining spl characters
						else {
							spl++;
						}
					} // for loop

				} // if
				in.close();
			} // while loop

			System.out.println("\nCount of Control characters: " + cc);
			System.out.println("Number of lower case count: " + lower);
			System.out.println("Number of Upper case count: " + upper);
			System.out.println("Count of Numbers: " + Numbers);
			System.out.println("Number of Special Characters: " + spl);

		} // try

		else {
			System.out.println("File Size cant Exceed 1GB!!");
		}
	}
}