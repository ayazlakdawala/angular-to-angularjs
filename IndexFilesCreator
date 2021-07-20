
import java.io.File;
import java.io.FileWriter;
import java.io.BufferedWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

public class IndexFilesCreator {

    private int indexCount = 0;
    private int moduleCount = 0;
    private int fileCount = 0;
    private int folderCount = 0;

    public static void main(String[] args) {
        CreateIndexFiles ft = new CreateIndexFiles();
        ft.run(args[0]);
    }

    public CreateIndexFiles() {
    }

    public void run(final String folderName) {

        System.out.println("");
        System.out.println("");
        System.out.println("Index.ts Creator");
        System.out.println("Processing Folder: " + folderName);


        final File folder = new File(folderName);
        try {
            listFilesForFolder(folder);
        } catch (IOException e) {
            System.out.println("*************************");
            System.out.println(e);
        }

        System.out.println("");
        System.out.println("");
        System.out.println("");
        System.out.println("      Index Files: " + this.indexCount);
        System.out.println("          Modules: " + this.moduleCount);
        System.out.println(" JavaScript Files: " + this.fileCount);
        System.out.println("          Folders: " + this.folderCount);
        System.out.println("");
    }

    private String chop(final String str) {
        return str.substring(0, str.length() - 3);

    }

    private void listFilesForFolder(final File folder) throws IOException {

        boolean hasModule = false;
        boolean hasFile = false;
        boolean hasDirectory = false;

        //System.out.println("Processing folder -> " + folder.getPath());
        System.out.print(".");

        //Process Child Folders
        for (final File fileEntry : folder.listFiles()) {
            if (!fileEntry.isDirectory()) {
                continue;
            }
            listFilesForFolder(fileEntry);
        }

        //Process this folder
        Writer writer = new Writer(folder.getPath() + "\\index.ts");

        //Look for module
        for (final File fileEntry : folder.listFiles()) {
            if (fileEntry.isDirectory()) {
                continue;
            }
//            if (!fileEntry.getName().endsWith(".ts") && !fileEntry.getName().endsWith(".js"))) {
//                continue;
//            }
            if (!fileEntry.getName().endsWith(".module.ts") &&
                    !fileEntry.getName().endsWith(".module.js")) {
                continue;
            }

            //System.out.println("   Module -> " + fileEntry.getName());
            writer.write("import './" + chop(fileEntry.getName()) + "'; \n");
            hasModule = true;
            this.moduleCount++;

        }

        if (hasModule) {
            writer.write("\n");
        }

        //List Files
        for (final File fileEntry : folder.listFiles()) {

            if (fileEntry.isDirectory()) {
                continue;
            }
            if (fileEntry.getName().endsWith(".module.ts")
                    || fileEntry.getName().endsWith(".module.js")
                    || fileEntry.getName().equals("index.ts")) {
                continue;
            }
            if (!fileEntry.getName().endsWith(".ts") &&
                    !fileEntry.getName().endsWith(".js")) {
                continue;
            }

            //System.out.println("   File -> " + fileEntry.getName());
            if (fileEntry.getName().endsWith(".downgrade.ts") ) {    //Skip new scripts
                writer.write("//import './" + chop(fileEntry.getName()) + "'; \n");
            } else {
                writer.write("import './" + chop(fileEntry.getName()) + "'; \n");
            }

            hasFile = true;
            this.fileCount++;
        }

        if (hasFile) {
            writer.write("\n");
        }

        //Process Child Folders
        for (final File fileEntry : folder.listFiles()) {
            if (!fileEntry.isDirectory()) {
                continue;
            }
            //listFilesForFolder(fileEntry);

            //Make sure a file was actually written

            //System.out.println(folder.getPath() + "/" + fileEntry.getName() + "/index.ts");
            if (!(new File(folder.getPath() + "/" + fileEntry.getName() + "/index.ts")).exists()) {
                continue;
            }

            writer.write("import './" + fileEntry.getName() + "/index'; \n");
            hasDirectory = true;
            this.folderCount++;
        }

        if (hasDirectory) {
            writer.write("\n");
        }

        //Done
        if (writer.close()) {
            this.indexCount++;
        }

    }

    private class Writer {

        private String filename;
        private BufferedWriter writer;

        public Writer(final String filename) {
            this.filename = filename;
        }

        private BufferedWriter getWriter() throws IOException {
            if (this.writer == null) {
                this.writer = new BufferedWriter(new FileWriter(this.filename));
            }
            return this.writer;
        }

        public void write(String str) throws IOException {
            this.getWriter().write(str);
        }

        public boolean close() throws IOException {
            if (this.writer != null) {
                this.writer.close();
                this.writer = null;
                return true;
            }
            return false;
        }

    }

}
